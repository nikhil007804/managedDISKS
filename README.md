 
# Azure Managed Disks Terraform Deployment
 
This repository contains Terraform code and a GitHub Actions workflow to deploy Azure Managed Disks resources.
 
---
 
## Prerequisites
 
- [Terraform](https://www.terraform.io/downloads.html) installed locally (for manual runs)
- An Azure subscription
- Service Principal credentials with sufficient permissions
- GitHub repository secrets configured:
  - `AZURE_CREDENTIALS`: JSON output from `az ad sp create-for-rbac --sdk-auth`
 
---
 
## Files
 
- `main.tf`: Main Terraform configuration for Azure resources
- `variables.tf`: Input variables for the deployment
- `output.tf`: Output values from the deployment
- `providers.tf`: Provider configuration (Azure)
- `terraform.tfvars`: Variable values
- `.github/workflows/deploy.yml`: GitHub Actions workflow for CI/CD
 
---
 
## Usage
 
### 1. Configure Secrets
 
Add your Azure credentials as a secret named `AZURE_CREDENTIALS` in your GitHub repository. This is typically generated using:
 
```bash
az ad sp create-for-rbac --sdk-auth > azure-credentials.json
```
 
---
 
### 2. Update `providers.tf` (Important!)
 
To fix the `subscription ID could not be determined` error, explicitly set the subscription ID in `providers.tf`:
 
```hcl
provider "azurerm" {
  features {}
  subscription_id = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  # Replace with your actual Subscription ID
}
```
 
You can get your Subscription ID with this command:
 
```bash
az account show --query id --output tsv
```
 
Alternatively, you can set it using environment variables:
 
```bash
export ARM_SUBSCRIPTION_ID="your-subscription-id"
export ARM_CLIENT_ID="your-client-id"
export ARM_CLIENT_SECRET="your-client-secret"
export ARM_TENANT_ID="your-tenant-id"
```
 
---
 
### 3. GitHub Actions Workflow
 
On every push to the `main` branch or on pull requests, the workflow will:
 
- Checkout the code
- Set up Terraform
- Log in to Azure using `AZURE_CREDENTIALS`
- Run `terraform init` and `terraform plan`
- On push to `main`, it will also run `terraform apply`
 
---
 
### 4. Manual Deployment (Local)
 
```bash
terraform init
terraform plan
terraform apply
```
 
Make sure you've run:
 
```bash
az login
az account set --subscription "your-subscription-id"
```
 
if you're running Terraform locally and not using environment variables.
 
---
 
## What are Azure Managed Disks?
 
Azure Managed Disks are storage drives provided by Microsoft Azure that you can use with virtual machines. They work like hard drives in the cloud. Azure takes care of managing these disks for you, so you don’t have to worry about storage accounts or hardware.
 
You can choose different types of disks based on your needs (for example, faster or cheaper). Managed Disks are reliable, secure, and easy to use for storing data in Azure.
 
---
 
Terraform | HashiCorp Developer
Explore Terraform product documentation, tutorials, and examples.
 
