# Azure Web Application on Azure VM (Mini Project)

## 🧭 Project Overview

This project demonstrates how to deploy a simple web application (Apache on Linux or IIS on Windows) on an Azure Virtual Machine. It includes:

* Provisioning an Azure VM
* Configuring Network Security Groups (NSGs)
* Installing and running a web server
* Enabling VM backups via Recovery Services Vault
* Monitoring with Azure Monitor and Log Analytics
* Optional automation via PowerShell

---

## 🧰 Tools & Services Used

* **Azure Virtual Machine** (Ubuntu or Windows Server)
* **Azure Network Security Group (NSG)**
* **Azure Recovery Services Vault**
* **Azure Monitor & Log Analytics**
* **PowerShell / Azure CLI**

---

## 🛠️ Project Folder Structure

```
azure-vm-webapp/
├── README.md
├── install-apache.sh           # Script to install Apache on Linux
├── install-iis.ps1             # Script to install IIS on Windows
├── create-resources.ps1        # Optional PowerShell script to deploy VM
├── architecture.png            # Architecture diagram 
```

---

## 🚀 Deployment Steps

### 1. Create Resource Group

```bash
az group create --name WebAppRG --location "East US"
```

### 2. Create Virtual Machine

#### Linux VM (Ubuntu)

```bash
az vm create \
  --resource-group WebAppRG \
  --name WebLinuxVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

#### Windows VM

```bash
az vm create \
  --resource-group WebAppRG \
  --name WebWindowsVM \
  --image Win2019Datacenter \
  --admin-username azureuser \
  --admin-password YourP@ssword123
```

### 3. Open Port 80 (HTTP)

```bash
az vm open-port --port 80 --resource-group WebAppRG --name WebLinuxVM
az vm open-port --port 80 --resource-group WebAppRG --name WebWindowsVM
```

### 4. Install Web Server

#### For Linux (Apache)

```bash
ssh azureuser@<public-ip>
sudo apt update && sudo apt install -y apache2
```

#### For Windows (IIS)

Use PowerShell on the VM:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

### 5. Enable VM Backup (Recovery Vault)

1. Go to Azure Portal
2. Create Recovery Services Vault
3. Go to the Vault → Backup → Select VM → Apply policy
4. Trigger a backup manually

### 6. Enable Monitoring

1. Create Log Analytics Workspace
2. Go to your VM → Monitoring → Diagnostic Settings
3. Send logs to Log Analytics
4. Run queries:

```kusto
Heartbeat | summarize LastSeen=max(TimeGenerated) by Computer
```

---

## 📸 Screenshot / Architecture Diagram

![deepseek_mermaid_20250603_aa044a](https://github.com/user-attachments/assets/50800e5f-90b5-425f-a746-3dd36a3ddcba)


---
## 📌 Cleanup (Important!)

```bash
az group delete --name WebAppRG --yes --no-wait
