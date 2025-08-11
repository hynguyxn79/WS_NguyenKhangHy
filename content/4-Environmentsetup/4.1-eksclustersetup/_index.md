---
title : "EKS Cluster Set Up"
date: "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---

In this section, we will create a Cluster file to describe the configuration of the EKS cluster in YAML format according to the eksctl standard. This helps you manage the cluster as **Infrastructure as Code (IaC)** instead of typing long commands in the CLI, with all parameters clearly defined in the file. It also ensures reusability and reproducibility: with just this file, you can recreate an identical cluster in any AWS environment.

**Step 1**: Create a text file on the desktop with the following content:

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

Save the file in **yaml** format and place it in the **eks-workshop** directory.

**Step 2**: Then, in the Terminal window, enter the command `eksctl create cluster -f cluster.yaml` to use **eksctl** to create an **Amazon EKS cluster** based on the configuration parameters defined in the **cluster.yaml** file.

![](/images/4.s3/1.png)

If the result appears as shown, you have successfully created the cluster.

**Step 3**: Use the command `kubectl get nodes` to check the **EKS cluster** after creation.

![](/images/4.s3/2.png)

If you see nodes like the ones above, it means you have successfully created the cluster !