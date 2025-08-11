+++
title = "Clean Up Resources"
date = "2025-06-14"
weight = 6
chapter = false
pre = "<b>6. </b>"
+++


We will perform the following steps to clean up all the resources created during this workshop.

### Delete EC2 Auto Scaling Group and Instances

1. Access [EC2 - Auto Scaling Group](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#AutoScalingGroups:)
+ Choose all created **Auto Scaling Group** .
+ Click **Action**.
+ Click **Delete**.

2. Access [EC2 - Instances](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:)
+ Choose 2 working Instance.
+ Click **Instance state**.
+ Click **Terminate (delete) instance**.

![](/images/6.clean/1.png)

### Delete AWS EKS

+ Access **EKS Console**
+ Click cluster **secure-networking**.
+ Click **Delete**.

![](/images/6.clean/2.png)

### Delete CloudWatch Log Group

+ Access **CloudWatch Console**.
+ Click **Log groups**.
+ Choose **/aws/container-insight/eks-workshop**.
+ Click **Actions** > **Delete log group**.

![](/images/6.clean/3.png)

### Delete VPC

Access **VPC Console** and delete in the following order:

**1. Subnets**
+ Choose all the **eksctl-secure-networking-cluster** subnets
+ Click **Action**.
+ Click **Delete subnet**.

**2. Route tables**
+ Chosee all the **eksctl-secure-networking-cluster** route tables.
+ Click **Action**.
+ Click **Delete route table**.

**3. Internet gateways**
+ Choose all the **eksctl-secure-networking-cluster** internet gateways.
+ Click **Action**.
+ Click **Delete internet gateway**.

**4. NAT gateways**
+ Choose all the **eksctl-secure-networking-cluster** NAT gateways.
+ Click **Action**.
+ Click **Delete NAT gateway**.

**5. Your VPCs**
+ Choose all the **eksctl-secure-networking-cluster** vpcs.
+ Click **Action**.
+ Click **Delete VPC**.