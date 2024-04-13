# CI/CD Pipeline for Azure with Terraform

This project is designed to explore how Azure and Terraform can be utilized to create a CI/CD pipeline, aiming to automate processes with minimal errors. The setup involves creating a robust Azure infrastructure including resource groups, storage accounts, and data factories, orchestrated using Terraform.

## Table of Contents

- [Introduction](#introduction)
- [System Architecture](#system-architecture)
- [Prerequisites](#prerequisites)
- [Installation Instructions](#installation-instructions)
- [Azure Infrastructure Automation](#azure-infrastructure-automation)
- [Storage Account Setup](#storage-account-setup)
- [Automating Azure Data Factory](#automating-azure-data-factory)
- [Testing and Validation](#testing-and-validation)
- [Destroying Resources](#destroying-resources)
- [Future Work](#future-work)
- [Contributing](#contributing)

## Introduction

This project uses Terraform to create a pipeline capable of managing an Azure account containing resource groups, data factory, and storage accounts.

![Project Overview](path_to_image_here)

## System Architecture

The architecture utilizes Azure services such as Resource Groups, Storage Accounts, and Data Factories:
- **Resource Groups** serve as units of deployment for managing resources.
- **Storage Accounts** store deployment artifacts and logs.
- **Data Factories** orchestrate data movement and transformation.

![System Architecture](path_to_image_here)

## Prerequisites

- Azure account and subscription
- Terraform installed
- Python editor (VS Code, PyCharm)
- Azure CLI linked to your Azure account

## Installation Instructions

1. Install Azure CLI and link it to your Azure account.
2. Install Terraform and set up your Python editor.

## Azure Infrastructure Automation

The Terraform configuration starts with initializing the Terraform provider using `terraform init`. The configuration details are defined in `main.tf` and variable values are stored in `variable.tf`.

![Terraform Initialization](path_to_image_here)

## Storage Account Setup

A Terraform module creates the storage account and its containers:
```hcl
resource "azurerm_storage_account" "storage" {
  name                   = var.storage_account_name
  resource_group_name    = var.resource_group_name
  location               = var.location
  account_tier           = "Standard"
  account_replication_type = "LRS"
  tags = {
    environment = "development"
  }
}
```
Additional containers and blobs within the storage account are set up to organize data effectively.

## Automating Azure Data Factory
Terraform scripts automate the setup of Azure Data Factory, linking it with the storage accounts and defining the data pipelines necessary for data movement:

```hcl
resource "azurerm_data_factory" "adf" {
  name                = var.data_factory_name
  resource_group_name = var.resource_group_name
  location            = var.location
}
```
## Testing and Validation
The configuration is applied using `terraform apply`, and results are validated in the Azure portal. This ensures that data is transferred correctly and all resources are configured as expected.

## Destroying Resources
To clean up resources, use:

```hcl
terraform destroy -var-file="variables.tfvars"
```

## Future Work
- Enhance the automation by including additional Azure services.
- Improve error handling and add more robust testing frameworks.

## Contributing
Contributions are welcome! Feel free to fork the repository, make changes, and submit a pull request. For major changes, please open an issue first to discuss what you would like to change.

```hcl
This README now includes placeholders for images corresponding to each major section, enhancing visual engagement and providing a clearer understanding of the project components and workflows. Replace `"path_to_image_here"` with the actual paths to your images when they are ready to be included.
```


