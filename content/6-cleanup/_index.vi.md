+++
title = "Dọn dẹp tài nguyên"
date = "2025-06-14"
weight = 6
chapter = false
pre = "<b>6. </b>"
+++


Chúng ta sẽ tiến hành các bước sau để xóa các tài nguyên chúng ta đã tạo trong bài thực hành này.

### Xóa EC2 Auto Scaling Group và Instances

1. Vào [EC2 - Auto Scaling Group](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#AutoScalingGroups:)
+ Chọn **Auto Scaling Group** đã tạo
+ Chọn **Action**.
+ Chọn **Delete**.

2. Vào [EC2 - Instances](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:)
+ Chọn 2 Instance đang hoạt động.
+ Chọn **Instance state**.
+ Chọn **Terminate (delete) instance**.

![](/images/6.clean/1.png)

### Xóa AWS EKS

+ Vào **EKS Console**
+ Chọn cluster **secure-networking**.
+ Chọn **Delete**.

![](/images/6.clean/2.png)

### Xóa CloudWatch Log Group

+ Vào **CloudWatch Console**.
+ Chọn **Log groups**.
+ Chọn nhóm **/aws/container-insight/eks-workshop**.
+ Chọn **Actions** > **Delete log group**.

![](/images/6.clean/3.png)

### Xóa VPC

Vào **VPC Console** và xóa theo thứ tự:

**1. Subnets**
+ Chọn các subnet có tên **eksctl-secure-networking-cluster**
+ Chọn **Action**.
+ Chọn **Delete subnet**.

**2. Route tables**
+ Chọn các Route tablesc có tên **eksctl-secure-networking-cluster**.
+ Chọn **Action**.
+ Chọn **Delete route table**.

**3. Internet gateways**
+ Chọn các Internet gateways có tên **eksctl-secure-networking-cluster**.
+ Chọn **Action**.
+ Chọn **Delete internet gateway**.

**4. NAT gateways**
+ Chọn NAT gateway có tên **eksctl-secure-networking-cluster**.
+ Chọn **Action**.
+ Chọn **Delete NAT gateway**.

**5. Your VPCs**
+ Chọn VPC có tên **eksctl-secure-networking-cluster**.
+ Chọn **Action**.
+ Chọn **Delete VPC**.
