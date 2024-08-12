# Simple DevOps Project: Creating a Static Website on Azure Storage with Terraform

## Overview

This project will guide you through creating an Azure Storage account, configuring it for static website hosting, and deploying a simple static website using Terraform.

## Prerequisites

- **Azure Account**: Ensure you have an active Azure subscription.
- **Terraform**: Install Terraform on your machine.
- **Azure CLI**: Install and configure the Azure CLI.

## Project Overview

- **Set up Terraform**: Configure Terraform to interact with Azure.
- **Create Azure Storage Account**: Use Terraform to create an Azure Storage account.
- **Enable Static Website Hosting**: Configure the storage account for static website hosting.
- **Deploy Website Files**: Upload the website files to the storage account.

## Step-by-Step Guide

### 1. Set Up Terraform

Create a new directory for your Terraform project and navigate into it:

```bash
mkdir terraform-azure-static-website
cd terraform-azure-static-website
```

Create a `provider.tf` and `variable.tf` file for your Terraform configuration:

```bash
touch main.tf
```

### 2. Configure Terraform Provider

In `provider.tf`, configure the Azure provider:

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "3.115.0"
    }
  }
}

provider "azurerm" {
  features {}  # Required but empty
}
```

### 3. Create an Azure Storage Account

Add the following code to `provider.tf` or `variable.tf` to create an Azure Storage account:

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "rg-static-website"
  location = "East US"
}

resource "azurerm_storage_account" "storage" {
  name                     = "mystorageaccountname"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"

  static_website {
    index_document    = "index.html"
    error_404_document = "404.html"
  }
}
```

### 4. Deploy Website Files

If you have a simple website (e.g., `index.html` and `404.html` files), you can upload these files to the storage account using Azure CLI or Terraform.

Here's how you can upload files using the Azure CLI:

```bash
az storage container create --name $web --account-name mystorageaccountname
```

```bash
az storage blob upload-batch \
  --account-name mystorageaccountname \
  --source ./website-files \
  --destination '$web'
```

### 5. Initialize and Apply Terraform Configuration

To deploy the resources on Azure, run the following commands:

```bash
terraform init
terraform plan
terraform apply
```

Type `yes` when prompted to confirm the creation of the resources.

### 6. Access Your Static Website

Once the deployment is complete, you can access your static website using the URL provided by Azure Storage. It typically looks like:

```http
http://mystorageaccountname.z22.web.core.windows.net
```

### Cleanup

To clean up the resources when you're done, run:

```bash
terraform destroy
```

This will delete all the resources you created with Terraform.

## Conclusion

This project gives you hands-on experience with Terraform and Azure. You'll understand how to create infrastructure as code, deploy resources to Azure, and host a static website. You can extend this project by adding CI/CD pipelines, custom domains, or SSL certificates for more advanced use cases.
