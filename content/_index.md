---
title : "Container Network Security"
date: "2025-06-14"
weight : 1 
chapter : false
---
# Container Networking Security on Amazon EKS using Calico and AWS VPC CNI

### Overview

In this lab, you will explore and practice advanced network security techniques in a **Kubernetes** environment running on **Amazon EKS**. By leveraging the combined capabilities of **Calico** and **AWS VPC CNI**, this workshop focuses on:

- Configuring and managing **Network Policies** to control traffic between pods and namespaces.

- Applying microsegmentation to isolate application components and minimize the attack surface.

- Implementing traffic encryption to protect data in transit within the cluster.

- Monitoring and analyzing network activities using **Fluent Bit**, **CloudWatch**, and **Grafana**.

![](/images/1.png) 

Participants will be guided step-by-step from provisioning the **EKS Cluster**, installing **Calico**, configuring the **AWS VPC CNI**, deploying security policies, testing and validating their effectiveness, to setting up a comprehensive monitoring and alerting system.

### Contents

 1. [Introduction](1-introduce/)
 2. [Preparation Steps](2-Prerequiste/)
 3. [Access to AWS Configure](3-Accessawsconfigure/)
 4. [Environment Set Up](4-Environmentsetup/)
 5. [Monitoring With Grafana](5-Grafana/)
 6. [Clean Up Resources](6-cleanup/)
