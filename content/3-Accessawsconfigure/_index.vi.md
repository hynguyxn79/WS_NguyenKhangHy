---
title : "Truy cập AWS Configure"
date: "2025-06-14"
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

Trong bước này, chúng ta sẽ thực hiện tạo kết nối đến AWS Console, dùng để thiết lập AWS CLI với thông tin xác thực và cấu hình mặc định, để sau đó bạn có thể chạy các lệnh AWS từ terminal.

**Bước 1**: Trong cửa sổ CMD, gõ lệnh `aws configure` để thiết lập AWS CLI với thông tin xác thực và cấu hình mặc định.

![](/images/3.connect/1.png)

**Bước 2**: Sau khi chạy lệnh `aws configure`, nó sẽ yêu cầu bạn nhập 4 thông tin:

- **AWS Access Key ID**: Nhập **Access Key ID** của tài khoản root hay IAM của bạn trên AWS Console (**VD: AKIAxxxxxxxxxxxx**).

- **AWS Secret Access Key**: Nhập **Secret Access Key** của tài khoản root hay IAM của bạn trên AWS Console (**VD: AbCdEfGhIjKlMnOpQrStUvWxYz**).

- **Default region name**: Nhập `ap-southeast-1` cho Singapore.

- **Default output format**: Nhập `json`.

**Bước 3**: Sau khi nhập xong thông tin, nhập lệnh `aws sts get-caller-identity` để kiểm tra thông tin bạn nhập đúng chưa.

![](/images/3.connect/2.png)

Nếu bạn nhập đúng, kết quả sẽ trả về như sau (ở đây máy dùng tài khoản root). Bạn dã hoàn công việc tạo kết nối đến AWS Console.