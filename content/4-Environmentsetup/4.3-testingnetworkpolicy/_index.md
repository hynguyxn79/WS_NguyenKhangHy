---
title : "Testing Networkpolicy"
date: "2025-06-14"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

**Step 1**: Create a test file named `pod a.yaml` and save it in the **eks-workshop** directory with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-a
  namespace: secure-test
  labels:
    app: a
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
In the CMD window, use the command `kubectl apply -f pod-a.yaml` to apply the newly created pod.

![](/images/4.s3/6.png)

**Step 2**: Next, create a test file named `pod b.yaml` and save it in the **eks-workshop** directory with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-b
  namespace: secure-test
  labels:
    app: b
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'sleep 3600']
```
In the CMD window, use the command `kubectl apply -f pod-b.yaml` to apply the newly created pod.

![](/images/4.s3/7.png)

**Step 3**: Check if pod-b can successfully access pod-a through the internal IP by running the command: `kubectl exec -n secure-test pod-b -- wget -qO- http://*IP of pod a*`

![](/images/4.s3/8.png)

If successful, the result will be as shown above.

**Step 4**: We will create a **NetworkPolicy** to deny all ingress traffic to **pod-a**, then verify that **pod-b** can no longer access it. Create a text file named `deny-all.yaml` in the **eks-workshop** directory with the following content:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-ingress
  namespace: secure-test
spec:
  podSelector:
    matchLabels:
      app: a
  policyTypes:
  - Ingress
```
Then, in the CMD window, use the command `kubectl apply -f deny-all.yaml` to apply the newly created `deny-all.yaml file`.

![](/images/4.s3/10.png)

**Step 5**: Recheck the connection between **pod-a** and **pod-b** using the command: `kubectl exec -n secure-test pod-b -- wget -T 5 -qO- http://*IP of pod a*`.

![](/images/4.s3/11.png)

If the result hangs or shows **wget: download timed out**, it means the NetworkPolicy has successfully blocked the connection!