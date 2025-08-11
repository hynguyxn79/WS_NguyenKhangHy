---
title : "Kiểm tra Networkpolicy"
date: "2025-06-14"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

**Bước 1**: Tạo một file test đặt tên là `pod a.yaml` lưu tại thư mục **eks-workshop** với nội dung:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-a
  namespace: secure-test
  labels:
    app: a
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
Tại cửa sổ CMD sử dụng lệnh `kubectl apply -f pod-a.yaml` để apply pod vừa mới tạo.

![](/images/4.s3/6.png)

**Bước 2**: Tiếp tục tạo một file test đặt tên là `pod b.yaml` lưu tại thư mục **eks-workshop** với nội dung:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-b
  namespace: secure-test
  labels:
    app: b
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'sleep 3600']
```
Tại cửa sổ CMD sử dụng lệnh `kubectl apply -f pod-b.yaml` để apply pod vừa mới tạo.

![](/images/4.s3/7.png)

**Bước 3** Kiếm tra pod-b đã truy cập thành công vào pod-a thông qua IP nội bộ chưa bằng lệnh `kubectl exec -n secure-test pod-b -- wget -qO- http://*Ip của pod a*`

![](/images/4.s3/8.png)

Nếu thành công kết quả sẽ như hình trên.

**Bước 4**: Chúng ta sẽ tạo một **NetworkPolicy** chặn tất cả ingress vào **pod-a**, kiểm tra **pod-b** không còn gọi được nữa. Tạo một file text tên là `deny-all.yaml` tại thư mục **eks-workshop** với nội dung như sau:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-ingress
  namespace: secure-test
spec:
  podSelector:
    matchLabels:
      app: a
  policyTypes:
  - Ingress
```
Sau đó tại cửa sổ CMD sử dụng lệnh `kubectl apply -f deny-all.yaml` để apply file `deny-all.yaml` vừa tạo.

![](/images/4.s3/10.png)

**Bước 5**: Kiểm tra lại kết nối giữa **pod a** và **pod b** bằng lệnh `kubectl exec -n secure-test pod-b -- wget -T 5 -qO- http://*Ip của pod a*`.

![](/images/4.s3/11.png)

Nếu kết quả bị treo hoặc **wget: download timed out** thì có nghĩa là Networkpolicy đã chặn kết nối thành công !