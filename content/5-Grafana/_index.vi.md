---
title : "Monitoring với Grafana"
date: "2025-06-14"
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

Trong phần này chúng ta sẽ cài đặt **Grafana** lên **EKS cluster secure-networking** bằng **Helm** để tích hợp **CloudWatch Logs**.

**Bước 1**: Cài **Helm** trên máy bạn bằng lệnh `choco install kubernetes-helm -y`.

![](/images/5.fwd/1.png)

**Bước 2**: Cài đặt **Grafana** trên **EKS** bằng **Helm**.

Tạo namespace monitoring bằng lệnh `kubectl create namespace monitoring`. 

![](/images/5.fwd/2.png)

Thêm **Helm** vào **Grafana** với lệnh `helm repo add grafana https://grafana.github.io/helm-charts`.

![](/images/5.fwd/3.png)

Dùng lệnh `helm repo update` đế cập nhật bản mới nhất cho **Helm**.

![](/images/5.fwd/4.png)

Dùng lệnh `helm install my-grafana grafana/grafana --namespace monitoring --set adminUser=admin --set adminPassword=securepassword --set service.type=ClusterIP` để cài **Grafana** không dùng volume (để tránh lỗi Pending). Trong đó **admin** / **securepassword** là tài khoản bạn sẽ dùng để đăng nhập giao diện **Grafana** sau này.

![](/images/5.fwd/5.png)

**Bước 3**: Mở **Grafana** trên trình duyệt
Chạy lệnh `kubectl port-forward -n monitoring service/my-grafana 3000:80` trong CMD (không đóng cửa sổ sau khi chạy):

![](/images/5.fwd/6.png)

Mở trình duyệt và truy cập **Grafana** bằng web theo địa chỉ `http://localhost:3000`. Sau đó nhập 

- Username: `admin`

- Password: `securepassword`

![](/images/5.fwd/7.png)

**Bước 4**: Vào **AWS Console IAM** tạo **IAM Use**r mới (nếu chưa có), với policy `CloudWatchReadOnlyAccess` và tạo access key (ID + Secret), sau đó ghi lại 2 thông tin:

- **Access Key ID**

- **Secret Access Key**

![](/images/5.fwd/8.png)

**Bước 5**: Tạo **Dashboard** trong **Grafana** để xem logs container (Calico và ứng dụng).

Ở góc phải, click vào biểu tượng **+** Chọn **New Dashboard**, rồi sau đó chọn **Add Visualization**, chọn datasource là **cloudwatch**.

![](/images/5.fwd/9.png)

![](/images/5.fwd/10.png)

![](/images/5.fwd/11.png)

**Bước 6**: Thiết lập **Dashboard** để xem logs.

Sau khi **Add Visualization**. Bên phần **Visualization** ta chọn kiểu bảng là **Logs** .

![](/images/5.fwd/12.png)

Ở phần **Cloudwatch Metrics**, ta chuyển thành **Cloudwatch Logs**.

![](/images/5.fwd/13.png)

Trong phần **Select Log Group** ta chọn **/aws/container-insight/eks-workshop** và chọn **Add log groups**

![](/images/5.fwd/14.png)

Trong ô **Query** sau khi đã bật Logs mode, bạn dán truy vấn sau tùy mục tiêu để xem logs từ **Calico**.

```
fields @timestamp, @logStream, @message
| filter @logStream like /calico/
| sort @timestamp desc
| limit 50
```

Sau đó ấn **Run queries** và các logs từ Calico sẽ hiện ra.

![](/images/5.fwd/16.png)

![](/images/5.fwd/15.png)

Vậy là chúng ta đã hoàn thành xong bài thực hành!