# Azure Secure Linux VM Deployment using Bicep
This project automates the deployment of a **secure Linux Virtual Machine** in Microsoft Azure using **Bicep (Infrastructure as Code)**.  
It provisions a complete environment including Virtual Network, Subnet, NSG, Public IP, NIC, and Linux VM — following Azure best practices.

## Project Objectives
- Deploy consistent and repeatable Azure infrastructure using Bicep  
- Implement secure access using SSH key authentication  
- Control inbound traffic using Network Security Group (NSG)  
- Learn real-world cloud Infrastructure as Code concepts


## Architecture Overview
The following resources are deployed:

- **Resource Group**
- **Virtual Network** (10.0.0.0/16)
- **Subnet** (10.0.1.0/24)
- **Network Security Group**
  - Allows SSH (Port 22)
- **Public IP (Standard – Static)**
- **Network Interface**
- **Ubuntu Linux VM**

## Logical Architecture Diagram
Azure Resource Group
│
├── Virtual Network (10.0.0.0/16)
│ └── Subnet (10.0.1.0/24)
│ └── Network Interface ─── Network Security Group
│ └── Allow SSH (22)
│
├── Public IP (Static - Standard)
│
└── Linux Virtual Machine (Ubuntu)

## Project Structure
azure-secure-vm-bicep
│
├── Screenshots --> Images from portal and VS code
├── main.bicep --> Main deployment template
├── README.md --> Project documentation
└── .gitignore --> Ignore sensitive files

## Deployment Steps

1. Create Resource Group
```powershell
az group create `
  --name rg-secure-vm-bicep `
  --location australiaeast

2. Get SSH Public Key
Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

3. Deploy Bicep deployment
$sshKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

az deployment group create `
  --name secureVmBicepDeploy `
  --resource-group rg-secure-vm-bicep `
  --template-file .\main.bicep `
  --parameters adminPublicKey="$sshKey" prefix="ramba-bicep4"
🔐 Security Considerations
✔️ Password login disabled (SSH only)
✔️ NSG restricts inbound traffic to required port (22)
✔️ Standard Public IP used (recommended by Azure)

🔎 Verify Deployment
Check VM Status
az vm get-instance-view `
 --resource-group rg-secure-vm-bicep `
 --name ramba-bicep4-vm `
 --query "instanceView.statuses[*].displayStatus" `
 -o tsv
Expected output:
Provisioning succeeded
VM running

🌐 Connect to VM
ssh azureuser@<PublicIP>

🧹 Cleanup (Avoid Costs)
To delete everything:
az group delete --name rg-secure-vm-bicep --yes --no-wait

📌 Key Learnings

Bicep simplifies Azure IaC deployment
Standard Public IP → better security and compliance
Secure SSH authentication best practice
Real-world Azure architecture design

👤 Author
This project is part of my Azure Cloud Engineering journey, where I am developing real-world, hands-on experience by designing and deploying secure cloud solutions on Azure.

🏷️ Tags
Azure • Bicep • Infrastructure as Code • Virtual Machine • Networking • Cloud Security
