---
title : "Deploy Network Security with Calico"
date: "2025-06-14"
weight : 2 
chapter : false
pre : " <b> 4.2 </b> "
---

In this step, we will install and configure Calico.

**Step 1**: Create a new text file named `installation.yaml` in the **eks-workshop** directory with the following content:

```yaml
apiVersion: operator.tigera.io/v1
kind: Installation
metadata:
  name: default
spec:
  kubernetesProvider: EKS
  cni:
    type: AmazonVPC
  calicoNetwork:
    bgp: Disabled
```

**Step 2**: In the CMD window, enter the command `kubectl apply -f installation.yaml` to apply the newly created **installation.yaml** file.

![](/images/4.s3/3.png)

If the installation is successful, the result will be: **installation.operator.tigera.io/default created**

**Step 3**: Use the command `kubectl get pods -n tigera-operator` to check whether the Operators are running (the status should be **Running** or **Completed**).

![](/images/4.s3/4.png)

**Step 4**: Use the command `kubectl get pods -n calico-system` to check if all Calico network services are ready. If the status is **Running**, it means the services are ready.

![](/images/4.s3/5.png)