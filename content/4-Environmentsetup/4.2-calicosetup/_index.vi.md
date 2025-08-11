---
title : "Triển Khai Bảo Mật Mạng Với Calico"
date: "2025-06-14"
weight : 2 
chapter : false
pre : " <b> 4.2 </b> "
---

Trong bước này, chúng ta sẽ cài đặt và thiết lập Calico.

**Bước 1**: Tạo một file text mới tên `installation.yaml` trong thư mục **eks-workshop** với nội dung sau:

```yaml
apiVersion: operator.tigera.io/v1
kind: Installation
metadata:
  name: default
spec:
  kubernetesProvider: EKS
  cni:
    type: AmazonVPC
  calicoNetwork:
    bgp: Disabled
```

**Bước 2**: Tại cửa sổ CMD, nhập dòng lệnh `kubectl apply -f installation.yaml` để áp dụng file **installation.yaml** vừa tạo.

![](/images/4.s3/3.png)

Nếu tải thành công kết quả sẽ là: **installation.operator.tigera.io/default created**

**Bước 3**: SỬ dụng lệnh `kubectl get pods -n tigera-operator` để giúp bạn xem các Operator đã chạy chưa (trạng thái phải là **Running** hoặc **Completed**).

![](/images/4.s3/4.png)

**Bước 4**: Sử dụng lệnh `kubectl get pods -n calico-system` để giúp bạn kiểm tra xem toàn bộ dịch vụ mạng của Calico đã sẵn sàng chưa. Nếu là **Running** có nghĩa là dịch vụ đã sẵn sàng.

![](/images/4.s3/5.png)
