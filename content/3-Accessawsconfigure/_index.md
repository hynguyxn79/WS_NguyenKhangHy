---
title : "Access to AWS Configure"
date: "2025-06-14"
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

In this step, we will create a connection to the AWS Console to configure AWS CLI with authentication credentials and default settings, allowing you to run AWS commands from the terminal.

**Step 1**: In the CMD window, type the command `aws configure` to set up AWS CLI with authentication credentials and default configuration.

![](/images/3.connect/1.png)

**Step 2**: After running the `aws configure` command, you will be prompted to enter the following 4 pieces of information:

- **AWS Access Key ID**: Enter the **Access Key ID** of your root or IAM account from the AWS Console (**e.g., AKIAxxxxxxxxxxxx**).

- **AWS Secret Access Key**: Enter the **Secret Access Key** of your root or IAM account from the AWS Console (**e.g., AbCdEfGhIjKlMnOpQrStUvWxYz**).

- **Default region name**: Enter `ap-southeast-1` for Singapore.

- **Default output format**: Enter `json`.

Step 3: After entering the information, run the command `aws sts get-caller-identity` to verify that the information you entered is correct.

![](/images/3.connect/2.png)

If you entered the information correctly, the result will be returned as shown (in this example, the machine is using a root account). You have now completed the process of creating a connection to the AWS Console.