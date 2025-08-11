---
title : "Container Network Security"
date: "2025-06-14"
weight : 1 
chapter : false
---
# Làm việc với Container Network Security - Calico & AWS VPC CNI

### Tổng quan

 Trong bài lab này, bạn sẽ tìm hiểu và thực hành các kỹ thuật bảo mật mạng nâng cao trong môi trường **Kubernetes** trên **Amazon EKS**. Thông qua sự kết hợp giữa **Calico** và **AWS VPC CNI**, workshop tập trung vào:

- Thiết lập và quản lý **Network Policy** để kiểm soát lưu lượng giữa các pod và namespace.

- Áp dụng microsegmentation để chia nhỏ và cô lập các thành phần ứng dụng, hạn chế phạm vi tấn công.

- Thực hiện traffic encryption nhằm bảo vệ dữ liệu truyền tải nội bộ.

- Giám sát và phân tích hoạt động mạng thông qua **FluentBit**, **CloudWatch** và **Grafana**.

![](/images/1.png) 

Người tham gia sẽ được hướng dẫn từng bước từ việc khởi tạo **EKS Cluster**, cài đặt **Calico**, cấu hình **AWS VPC CNI**, triển khai các chính sách bảo mật, kiểm tra và xác minh hiệu quả, cho đến thiết lập hệ thống giám sát và cảnh báo.

### Nội dung

 1. [Giới thiệu](1-introduce/)
 2. [Các bước chuẩn bị](2-Prerequiste/)
 3. [Truy cập AWS Configure](3-Accessawsconfigure/)
 4. [Thiết lập môi trường](4-Environmentsetup/)
 5. [Monitoring với Grafana](5-Grafana/)
 6. [Dọn dẹp tài nguyên](6-cleanup/)
