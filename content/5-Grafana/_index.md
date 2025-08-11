---
title : "Monitoring With Grafana"
date: "2025-06-14"
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

In this section, we will install **Grafana** on the **EKS cluster secure-networking** using Helm to integrate **CloudWatch Logs**.

**Step 1**: Install **Helm** on your machine with the command: `choco install kubernetes-helm -y`

![](/images/5.fwd/1.png)

**Step 2**: Install **Grafana** on **EKS** using **Helm**.

Create the monitoring namespace with the command: `kubectl create namespace monitoring`

![](/images/5.fwd/2.png)

Add the **Grafana Helm** repository with the command: `helm repo add grafana https://grafana.github.io/helm-charts`

![](/images/5.fwd/3.png)

Use the command `helm repo update` to update **Helm** to the latest version.

![](/images/5.fwd/4.png)

Use the command: `helm install my-grafana grafana/grafana --namespace monitoring --set adminUser=admin --set adminPassword=securepassword --set service.type=ClusterIP` to install **Grafana** without using a volume (to avoid Pending errors). Here, **admin** / **securepassword** are the credentials you will use to log in to the **Grafana** interface later.

![](/images/5.fwd/5.png)

**Step 3**: Open **Grafana** in your browser
Run this command `kubectl port-forward -n monitoring service/my-grafana 3000:80` in the CMD window (do not close the window after running):

![](/images/5.fwd/6.png)

Open your browser and access **Grafana** at the URL `http://localhost:3000`. Then enter:

- Username: `admin`

- Password: `securepassword`

![](/images/5.fwd/7.png)

**Step 4**: In the **AWS Console IAM**, create a new **IAM User** (if not already created) with the **CloudWatchReadOnlyAccess** policy, and generate an access key (ID + Secret). Then record the following two pieces of information:

- **Access Key ID**

- **Secret Access Key**

![](/images/5.fwd/8.png)

**Step 5**: Create a **Dashboard** in **Grafana** to view container logs (Calico and applications).

At the top-right corner, click the **+** icon, select **New Dashboard**, then choose **Add Visualization**, and select **cloudwatch** as the datasource.

![](/images/5.fwd/9.png)

![](/images/5.fwd/10.png)

![](/images/5.fwd/11.png)

**Step 6**: Configure the **Dashboard** to view logs.

After **Adding Visualization**, under the **Visualization** section, select the table type as **Logs**.

![](/images/5.fwd/12.png)

In the **CloudWatch Metrics** section, switch to **CloudWatch Logs**.

![](/images/5.fwd/13.png)

In the **Select Log Group** section, choose **/aws/container-insight/eks-workshop** and click **Add log groups**.

![](/images/5.fwd/14.png)

In the **Query** box, after enabling Logs mode, paste the following query depending on your target to view logs from **Calico**.

```
fields @timestamp, @logStream, @message
| filter @logStream like /calico/
| sort @timestamp desc
| limit 50
```

Then click **Run queries** and the logs from Calico will appear.

![](/images/5.fwd/16.png)

![](/images/5.fwd/15.png)

So we have finished the lab !