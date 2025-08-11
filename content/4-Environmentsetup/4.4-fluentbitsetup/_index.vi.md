---
title : "Thiết lập FluentBit"
date: "2025-06-14"
weight : 4 
chapter : false
pre : " <b> 4.4 </b> "
---

**Bước 1**: vào **AWS Console** , vào **IAM**, chọn phần **Policies** và chọn **Create policy**

![](/images/4.s3/12.png)

Tại phần **Create policy** chọn kiểu file **Json** và dán dòng lệnh này vào:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:PutLogEvents",
        "logs:DescribeLogStreams",
        "logs:DescribeLogGroups",
        "logs:CreateLogStream",
        "logs:CreateLogGroup"
      ],
      "Resource": "*"
    }
  ]
}
```
Sau đó ấn **Next**, đặt tên cho policy là `FluentBitCloudWatchPolicy` sau đó chọn **Create policy**.

![](/images/4.s3/13.png)


**Bước 2**: Truy cập **EC2** trong AWS Console , chọn **Instances** và chọn instance đầu tiên.

![](/images/4.s3/14.png)

Trong tab **Security**, nhìn phần **IAM Role** và click vào tên Role đó để mở **IAM Console**

![](/images/4.s3/15.png)

Ở Console của **IAM Roles**, ở phần **Add permissions** chọn **Attach policies** và sau đó tìm policy `FluentBitCloudWatchPolicy` và sau đó tick vào policy và chọn **Add permissions**.

![](/images/4.s3/16.png)

Hiện tại IAM Role của **EC2 node** đã có quyền ghi log lên **CloudWatch Logs**.

**Bước 3**: Tại thư mục eks-workshop, tạo 2 thư mục text sau:

1. `fluent-bit-configmap.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluent-bit-config
  namespace: amazon-cloudwatch
data:
  fluent-bit.conf: |
    [SERVICE]
        Flush        1
        Log_Level    info
        Parsers_File parsers.conf
        HTTP_Server  On
        HTTP_Listen  0.0.0.0
        HTTP_Port    2020

    [INPUT]
        Name              tail
        Path              /var/log/containers/*.log
        Tag               kube.*
        Refresh_Interval  5
        Rotate_Wait       30
        Mem_Buf_Limit     50MB
        Skip_Long_Lines   On
        DB                /var/log/flb_kube.db

    [FILTER]
        Name                kubernetes
        Match               kube.*
        Kube_URL            https://kubernetes.default.svc:443
        Merge_Log           On
        K8S-Logging.Parser  On
        K8S-Logging.Exclude On

    [OUTPUT]
        Name            cloudwatch_logs
        Match           kube.*
        region          ap-southeast-1
        log_group_name  /aws/container-insight/eks-workshop
        log_stream_prefix from-fluent-bit-
        auto_create_group true

  parsers.conf: |
    [PARSER]
        Name        docker
        Format      json
        Time_Key    time
        Time_Format %Y-%m-%dT%H:%M:%S.%L
        Time_Keep   On
```

2. `fluent-bit-daemonset.yaml`
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluent-bit
  namespace: amazon-cloudwatch
  labels:
    k8s-app: fluent-bit
spec:
  selector:
    matchLabels:
      k8s-app: fluent-bit
  template:
    metadata:
      labels:
        k8s-app: fluent-bit
    spec:
      serviceAccountName: fluent-bit
      tolerations:
        - operator: Exists
      containers:
        - name: fluent-bit
          image: public.ecr.aws/aws-observability/aws-for-fluent-bit:latest
          ports:
            - containerPort: 2020
              name: api
              protocol: TCP
          volumeMounts:
            - name: varlog
              mountPath: /var/log
            - name: config-volume
              mountPath: /fluent-bit/etc/
      volumes:
        - name: varlog
          hostPath:
            path: /var/log
        - name: config-volume
          configMap:
            name: fluent-bit-config
```
Ở cửa sổ CMD, sử dụng lệnh `kubectl create namespace amazon-cloudwatch` để tạo **namespace** cho **CloudWatch**.

![](/images/4.s3/i17.png)

Tiếp tục sử dụng 2 lệnh sau để apply cho 2 file **fluent-bit-configmap.yaml** và **fluent-bit-daemonset.yaml** vừa mới tạo ở trên

`kubectl apply -f fluent-bit-configmap.yaml`
`kubectl apply -f fluent-bit-daemonset.yaml`

![](/images/4.s3/18.png)

![](/images/4.s3/19.png)

**Bước 4**: Bật **OIDC Provider** cho **EKS cluster** bằng lệnh sau `eksctl utils associate-iam-oidc-provider --region=ap-southeast-1 --cluster=secure-networking --approve`. Lệnh này sẽ Tạo **OIDC identity provider** cho cụm và Cho phép bạn gán **IAM Role** cho **ServiceAccount** trong **Kubernetes**.

![](/images/4.s3/20.png)

Sau đó chạy lại lệnh `eksctl create iamserviceaccount --name fluent-bit --namespace amazon-cloudwatch --cluster secure-networking --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy --override-existing-serviceaccounts --approve` để tạo **IAM ServiceAccount Fluent Bit**.

![](/images/4.s3/21.png)

**Bước 5**: Truy cập **AWS CloudWatch**, tìm log group như **/aws/container-insight/eks-workshop**, bạn sẽ thấy các log đang hoạt động.

![](/images/4.s3/22.png)
