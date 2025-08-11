---
title : "FluentBit Set Up"
date: "2025-06-14"
weight : 4 
chapter : false
pre : " <b> 4.4 </b> "
---

**Step 1**: In the **AWS Console**, go to **IAM**, select **Policies**, and click **Create policy**.

![](/images/4.s3/12.png)

In the **Create policy** section, select the **Json** tab and paste the following policy document:

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
Then click **Next**, name the policy `FluentBitCloudWatchPolicy`, and click **Create policy**.

![](/images/4.s3/13.png)


**Step 2**: In the **AWS Console**, go to **EC2**, select **Instances**, and choose the first instance.

![](/images/4.s3/14.png)

In the **Security** tab, locate the **IAM Role** section and click on the role name to open the **IAM Console**.

![](/images/4.s3/15.png)

In the **IAM Roles** console, under *Add permissions*, select **Attach policies**, then search for the `FluentBitCloudWatchPolicy`. Check the box next to the policy and click **Add permissions**.

![](/images/4.s3/16.png)

The **EC2 node** IAM Role now has permission to write logs to **CloudWatch Logs**.

**Step 3**: In the **eks-workshop** directory, create the following two text directories:

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
In the CMD window, use the command `kubectl create namespace amazon-cloudwatch` to create a namespace for **CloudWatch**.

![](/images/4.s3/i17.png)

Next, use the following two commands to apply the two files **fluent-bit-configmap.yaml** and **fluent-bit-daemonset.yaml** that were just created above:

`kubectl apply -f fluent-bit-configmap.yaml`
`kubectl apply -f fluent-bit-daemonset.yaml`

![](/images/4.s3/18.png)

![](/images/4.s3/19.png)

**Step 4**: Enable the OIDC Provider for the EKS cluster using the following command: `eksctl utils associate-iam-oidc-provider --region=ap-southeast-1 --cluster=secure-networking --approve`. This command creates an **OIDC identity provider** for the cluster and allows you to assign **IAM Roles** to **ServiceAccounts** within **Kubernetes**.


![](/images/4.s3/20.png)

Then run the command: `eksctl create iamserviceaccount --name fluent-bit --namespace amazon-cloudwatch --cluster secure-networking --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy --override-existing-serviceaccounts --approve` to create the **IAM ServiceAccount Fluent Bit**.


![](/images/4.s3/21.png)

**Step 5**: Access **AWS CloudWatch**, find the log group such as **/aws/container-insight/eks-workshop**, where you will see the active logs.

![](/images/4.s3/22.png)