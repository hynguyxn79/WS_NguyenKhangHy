---
title : "Giới thiệu"
date: "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

Trong kiến trúc hiện đại dựa trên microservices và container, việc đảm bảo bảo mật mạng (network security) là một yếu tố sống còn để ngăn chặn các rủi ro như truy cập trái phép, dò quét nội bộ hoặc nghe lén dữ liệu. Trong môi trường **Kubernetes**, nơi hàng trăm đến hàng ngàn container có thể tương tác lẫn nhau, việc kiểm soát luồng lưu lượng nội bộ trở nên rất phức tạp. Nếu không có các chính sách mạng phù hợp, một lỗ hổng nhỏ có thể dẫn đến hậu quả nghiêm trọng trên toàn hệ thống.

**Amazon EKS (Elastic Kubernetes Service)** là một dịch vụ được quản lý giúp triển khai **Kubernetes** một cách dễ dàng, bảo mật và mở rộng. **EKS** được tích hợp sẵn nhiều công cụ bảo mật mạng, và đặc biệt hỗ trợ các plugin như Calico để triển khai các chính sách mạng tiên tiến.

Trong **Amazon EKS**, networking được xử lý mặc định thông qua **AWS VPC CNI plugin**, giúp mỗi Pod có thể sử dụng địa chỉ IP thật từ VPC, đảm bảo khả năng giao tiếp trực tiếp và bảo mật theo chuẩn hạ tầng AWS. Việc kết hợp **VPC CNI** với **Calico** cho phép tận dụng ưu điểm của cả bảo mật mạng truyền thống (Security Groups) và kiểm soát traffic nội bộ sâu hơn (Network Policies).

---

## Mục tiêu chính

- **Triển khai AWS VPC CNI**: hiểu rõ cách thức hoạt động của **VPC CNI** trong việc cấp phát IP cho Pod, tích hợp với Security Groups và đóng vai trò nền tảng cho microsegmentation trong **EKS**.

- **Triển khai Network Policies**: sử dụng **Calico** để định nghĩa các chính sách kiểm soát traffic giữa các Pod. Giúp hạn chế phạm vi ảnh hưởng khi có container bị tấn công.

- **Thực hành Microsegmentation**: phân chia mạng nội bộ theo namespace và chính sách ingress/egress. Đây là chiến lược chia nhỏ bề mặt tấn công nhằm cô lập sự cố, giảm thiểu rủi ro lây lan.

- **Triển khai Traffic Encryption**: cấu hình chế độ policy-only và áp dụng mã hóa lưu lượng giữa các Pod trong cluster để ngăn chặn sniffing và MITM (Man-in-the-middle attack).

- **Thiết lập Monitoring**: sử dụng **Fluent Bit** để thu thập log từ container, chuyển về **Amazon CloudWatch Logs**. Đồng thời tích hợp với **Grafana** để trực quan hóa và tạo cảnh báo về hoạt động bất thường, đặc biệt là log lỗi từ **Calico**.

Ngoài ra, workshop còn cung cấp trải nghiệm thực tế trong việc xử lý sự cố DNS nội bộ, xác thực IAM cho Fluent Bit, sử dụng Helm để cài đặt Grafana và cuối cùng là dọn dẹp toàn bộ tài nguyên đã triển khai để tránh phát sinh chi phí không cần thiết.

---

## Mục tiêu cuối cùng

Giúp người học hiểu rõ cơ chế bảo vệ mạng nội bộ trong **Amazon EKS**, từ kiểm soát truy cập đến phát hiện rò rỉ lưu lượng, góp phần xây dựng một kiến trúc **Kubernetes** bảo mật, linh hoạt và có thể mở rộng trong môi trường doanh nghiệp.

