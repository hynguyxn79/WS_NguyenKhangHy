---
title: "Install Chocolatey"
date: "2025-06-14"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

**Install Chocolatey** for Windows to install required services such as **AWSCLI**, **KUBECTL**, **EKSCTL**.

**Step 1**: Use **Windows Powershell** running as **Administrator**, then execute the installation command for **Chocolatey** as shown below.

![](/images/2.prerequisite/1.png)

**Step 2**: Enter the following commands in order:
- `Set-ExecutionPolicy Bypass -Scope Process -Force` : This command allows **PowerShell** to run the **Chocolatey** installation script without being blocked by the default execution policy.

- `[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072` : This command ensures that **PowerShell** can use **TLS 1.2** to download files from the internet, as many servers (including **Chocolatey**) no longer accept **TLS 1.0** or **1.1**.

- `iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))` : This command downloads the **Chocolatey** installation script from the official site and executes it immediately.

When the installation is successful, the output will be as follows:

![](/images/2.prerequisite/2.png)