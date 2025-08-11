---
title : "Introduction"
date: "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

In a modern microservices and container-based architecture, ensuring network security is a critical factor in preventing risks such as unauthorized access, internal reconnaissance, or data sniffing. In a **Kubernetes** environment, where hundreds or even thousands of containers may interact with each other, controlling internal traffic flows becomes highly complex. Without appropriate network policies, a single vulnerability could lead to serious consequences across the entire system.

Amazon EKS (Elastic Kubernetes Service) is a managed service that enables secure, scalable, and simplified Kubernetes deployments. EKS comes with built-in network security tools and supports advanced plugins such as Calico for implementing fine-grained network policies.

**Amazon EKS** uses the **AWS VPC CNI plugin** by default, which assigns VPC IP addresses directly to Pods. This enables native communication within the VPC and integration with AWS-level network security tools such as Security Groups and route tables. When combined with **Calico**, it allows deeper traffic control through Kubernetes Network Policies while maintaining the scalability and reliability of AWS networking.

---

## Main Objectives

- **Deploy AWS VPC CNI**: Learn how the **VPC CNI** plugin allocates Pod IPs from the VPC subnet, integrates with Security Groups, and serves as the foundational layer for secure and scalable Pod networking in **EKS**.

- **Deploy Network Policies**: Use **Calico** to define traffic control rules between Pods. This helps limit the blast radius in case a container is compromised.

- **Practice Microsegmentation**: Segment internal networks by namespace and define specific ingress/egress policies. This strategy reduces the attack surface and helps isolate incidents.

- **Deploy Traffic Encryption**: Configure policy-only mode and apply encryption to traffic between Pods within the cluster to prevent sniffing and MITM (Man-in-the-middle) attacks.

- **Set up Monitoring**: Use **Fluent Bit** to collect container logs and send them to **Amazon CloudWatch Logs**. Integrate with **Grafana** to visualize and create alerts for suspicious activities—especially error logs from **Calico**.

Additionally, the workshop provides hands-on experience with troubleshooting internal DNS issues, configuring IAM authentication for Fluent Bit, using Helm to install Grafana, and finally cleaning up all deployed resources to avoid unnecessary costs.

---

## Final Goal

Equip learners with a solid understanding of network protection mechanisms in **Amazon EKS**, from access control to traffic leak detection — contributing to building a secure, scalable, and flexible **Kubernetes** architecture in enterprise environments.