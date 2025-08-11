---
title: "Cài đặt Chocolatey"
date: "2025-06-14"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

Cài đặt **Chocolatey** cho Windows để cài đặt các dịch vụ cần thiết như **AWSCLI**, **KUBECTL**, **EKSCTL**.

**Bước 1**: Sử dụng **Powershell** của Windows chạy dưới quyến **Administrator**.

![](/images/2.prerequisite/1.png)


**Bước 2**: Nhập theo thứ tự: 
- `Set-ExecutionPolicy Bypass -Scope Process -Force` : Đoạn lệnh này cho phép **PowerShell** chạy script cài đặt **Chocolatey** mà không bị chặn bởi chính sách mặc định.

- `[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072`: Đoạn lệnh này đảm bảo **PowerShell** có thể dùng **TLS 1.2** để tải file từ internet, vì nhiều server (trong đó có **Chocolatey**) không chấp nhận **TLS 1.0** hoặc **1.1** nữa.

- `iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))`: đoạn lệnh này tải script cài đặt **Chocolatey** từ trang chính thức và chạy nó luôn

Khi cài đặt thành công kết quả trả về như sau:

![](/images/2.prerequisite/2.png)

