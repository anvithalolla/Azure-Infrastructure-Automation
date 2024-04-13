# CI/CD Pipeline for Azure with Terraform

This project creates a robust CI/CD pipeline using Azure and Terraform, focusing on automating the setup and management of a suite of Azure resources. The goal is to provide a comprehensive, error-minimized automated system that can deploy and manage complex cloud infrastructures efficiently. This project involves creating a pipeline using Terraform, which automates the provisioning of an Azure account that includes key components such as resource groups, storage accounts, and a data factory. This setup demonstrates the utility of Infrastructure as Code (IaC) in managing and deploying cloud resources predictably and at scale.

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
- **Resource Groups** Serve as containers for grouping related resources for easy management, access control, and cost reporting.
- **Storage Accounts** Used for storing CI/CD artifacts such as build outputs, logs, and binaries. They are configured to use blob storage for unstructured data, accessible globally via HTTP/HTTPS.
- **Data Factories** Facilitates data integration services that allow data-driven workflow creation for orchestrating and automating data movement and data transformation.

![System Architecture](path_to_image_here)

## Prerequisites

- Azure account and subscription
- Terraform installed
- Python editor (VS Code, PyCharm)
- Azure CLI linked to your Azure account

## Installation Instructions

1. `Azure CLI`
2. `Terraform`

## Azure Infrastructure Automation

The Terraform configuration starts with initializing the Terraform provider using `terraform init`. The configuration details are defined in `main.tf` and variable values are stored in `variable.tf`.

![Terraform Initialization](path_to_image_here)

## Resource Group Configuration
The resource group is defined in Terraform with specifications for name, location, and tags. This is crucial for organizing resources within Azure:

```hcl
resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = var.location
  tags     = {
    environment = "development"
  }
}
```

## Storage Account Setup

Within the resource group, a storage account is created. This account is critical for storing various CI/CD artifacts:

```hcl
resource "azurerm_storage_account" "storage" {
  name                    = var.storage_account_name
  resource_group_name     = azurerm_resource_group.rg.name
  location                = var.location
  account_tier            = "Standard"
  account_replication_type = "LRS"
}
  }
}
```
Additional containers and blobs within the storage account are set up to organize data effectively.

```hcl
resource "azurerm_storage_container" "container" {
  name                  = "containername"
  storage_account_name  = azurerm_storage_account.storage.name
  container_access_type = "private"
}

resource "azurerm_storage_blob" "blob" {
  name                   = "test.txt"
  storage_account_name   = azurerm_storage_account.storage.name
  storage_container_name = azurerm_storage_container.container.name
  type                   = "Block"
  source_content         = "Hello! Welcome to my project"
}
```



## Automating Azure Data Factory
Terraform scripts automate the setup of Azure Data Factory, linking it with the storage accounts and defining the data pipelines necessary for data movement:

```hcl
resource "azurerm_data_factory" "adf" {
  name                = "example-adf"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_data_factory_linked_service_azure_blob_storage" "adf_ls" {
  name            = "example-linked-service"
  resource_group_name = azurerm_resource_group.rg.name
  data_factory_name   = azurerm_data_factory.adf.name
  url                = azurerm_storage_account.storage.primary_blob_endpoint
}

resource "azurerm_data_factory_pipeline" "adf_pipeline" {
  name                = "example-pipeline"
  resource_group_name = azurerm_resource_group.rg.name
  data_factory_name   = azurerm_data_factory.adf.name
  activities_json     = <<JSON
  [
    {
      "name": "Copy Data",
      "type": "Copy",
      "inputs": [{ "referenceName": "input-dataset", "type": "DatasetReference" }],
      "outputs": [{ "referenceName": "output-dataset", "type": "DatasetReference" }]
    }
  ]
  JSON
}

```
## Testing and Validation
Post-deployment, the pipeline is tested to ensure all resources are correctly provisioned and functioning as expected. This includes verifying data movement through Data Factory pipelines and accessing blobs within the storage account.

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

```
This README now includes placeholders for images corresponding to each major section, enhancing visual engagement and providing a clearer understanding of the project components and workflows. Replace `"path_to_image_here"` with the actual paths to your images when they are ready to be included.
```


