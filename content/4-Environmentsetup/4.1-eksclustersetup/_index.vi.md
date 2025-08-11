---
title : "Thiết lập EKS Cluster"
date: "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---

Trong phần này, chúng ta sẽ tạo một file Cluster để mô tả cấu hình của EKS cluster theo chuẩn eksctl dưới dạng YAML. Giúp bạn quản lý cluster như một **infrastructure as code (IaC)** thay vì phải gõ lệnh dài trên CLI, mọi thông số được ghi rõ trong file. Đồng thời dảm bảo tái sử dụng và tái tạo: chỉ cần file này là có thể tạo lại cluster y hệt trên bất kỳ môi trường AWS nào.

**Bước 1**: Tạo một file text ở deskstop với nội dung như sau:

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: secure-networking
  region: ap-southeast-1

nodeGroups:
  - name: ng-1
    instanceType: t3.medium
    desiredCapacity: 2
    volumeSize: 20
```

Lưu file với dạng **yaml** và lưu vào thư mục **eks-workshop**.

**Bước 2**: Sau đó ở cửa sổ Terminal ta nhập dòng lệnh `eksctl create cluster -f cluster.yaml` để dùng **eksctl** để tạo một **Amazon EKS cluster** dựa trên thông số cấu hình được định nghĩa trong file **cluster.yaml**

![](/images/4.s3/1.png)

Nếu kết quả như sau là bạn đã tạo thành công cluster.

**Bước 3**: Sử dụng lệnh `kubectl get nodes` để kiểm tra **cụm EKS** sau khi tạo.

![](/images/4.s3/2.png)

Nếu bạn thấy các node như trên có nghĩa là bạn đã tạo thành công !