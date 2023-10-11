---
title: "Terraform With Azure DevOps"
summary: "Want to setup Azure DevOps to orchestrate your Terraform deployments to Azure? Then this is the blog post for you!"
date: 2021-09-10
draft: false
categories: ["DevOps"]
tags: ["Azure", "How To", "Terraform", "DevOps", "Pipelines", "CI/CD", "Azure DevOps", "Enterprise Scale"]
thumbnail: "/img/thumbs/tf-azdo-thumb.png"

twitter:
  card: "summary"
  site: "@Jack_Ref"
  title: "Terraform With Azure DevOps"
  description: "Want to setup Azure DevOps to orchestrate your Terraform deployments to Azure? Then this is the blog post for you!"
  image: "https://jacktracey.co.uk/img/thumbs/tf-azdo-thumb.png"
---
## Introduction

Over the past few weeks I have been helping a couple of my customers take their first steps into the world of DevOps and Infrastructure-as-Code with Terraform.

It's safe to say there is an array of fantastic content out there already in the community and from the vendors themselves, which have certainly been useful to use with my customers. Some of the content I have been using with my customers is listed below:

- [Azure Docs - Terraform on Azure](https://docs.microsoft.com/en-gb/azure/developer/terraform/overview)
- [Hashicorp Terraform Docs - Get Started Terraform on Azure](https://learn.hashicorp.com/collections/terraform/azure-get-started)
- [Thomas Thornton - Terraform With Azure DevOps and Build Artifacts](https://thomasthornton.cloud/2020/11/02/deploying-terraform-using-azure-devops-with-build-artifacts/)

> **Important:** Azure DevOps made changes to free pipeline grants for new organisations back in March 2021, which means you may need to apply to be able to gain access to the free minutes for pipeline agent runs. To do this review the steps instructions and guidance published [here.](https://docs.microsoft.com/azure/devops/release-notes/2021/sprint-184-update#changes-to-azure-pipelines-free-grants) 

## So What Is This Blog Post About?

Good question. In this blog post I want to share with you how I configure Azure DevOps (Project, Repos, Pipelines, Artifacts, Branch Policies, Variable Groups, Service Connections etc.) to deploy Terraform into Azure. I'll also show you how I configure Azure resources like Storage Accounts, Key Vaults & Service Principals to handle the remote state for Terraform with Azure DevOps and handling access secrets securely.

> This not only applies to Azure and could also re-purposed very easily for any Terraform style deployment using Azure DevOps to orchestrate the process.

Now I won't be doing this as a long blog post, as many of my other posts are, instead I have recorded this all as a video and put on YouTube for you all to enjoy instead!

I've also created an overview diagram to explain the logical flow from someone creating Terraform code to deployment. Which will hopefully help everyone understand all the pieces involved in this process!

So with that, checkout the below diagram, video walk through and beneath all that code snippets and example Terraform files; enjoy!

## Process Flow Diagram 

{{< rawhtml >}}

<a href="/img/tf-azdo-flow-diagram.png" target="_"> <img src="/img/tf-azdo-flow-diagram.png"> </a>

{{< /rawhtml >}}
> **Click on the diagram above to open it in full in another tab**

The diagram is also available below in Visio & PDF formats:
- [Visio](https://jtjsacpublic.blob.core.windows.net/blogsharedfiles/JT-TF-AzDo-Diagram.vsdx)
- [PDF](https://jtjsacpublic.blob.core.windows.net/blogsharedfiles/JT-TF-AzDo-Diagram.pdf)

## Video Walk Through

{{< youtube AWXOYS-SBfY >}}

## Code Snippets

> A YAML extracted version of all of the pipelines shown in the video are available for you to review and import below, if desired. If not, you can copy and paste paths etc. 
>  
> They are available from: [https://github.com/jtracey93/PublicScripts/tree/master/AzureDevOps/TerraformDemoPipelines/ES-Terraform-Community](https://github.com/jtracey93/PublicScripts/tree/master/AzureDevOps/TerraformDemoPipelines/ES-Terraform-Community)
>  
> Import instruction can be found here: [https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/clone-import-pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/clone-import-pipeline?view=azure-devops&tabs=yaml)

#### Terraform Plan Build Pipeline

**Terraform Init**
```go {linenos=false} 
terraform init -backend-config="access_key=$(NAME OF KEY VAULT SECRET FOR STORAGE ACCOUNT KEY)"
```

**Terraform Validate**
```go {linenos=false} 
terraform validate
```

**Terraform Plan**
```go {linenos=false} 
terraform plan -input=false -out=tfplan -var="spn-client-id=$(CHANGEME-spn-client-id)" -var="spn-client-secret=$(CHANGEME-spn-secret)" -var="spn-tenant-id=$(CHANGEME-spn-tenant-id)"
```

**Create Archive Step**
```go {linenos=false} 
$(Build.ArtifactStagingDirectory)/$(Build.BuildId)-tfplan.tgz
```

**Publish Artifact Step**
```go {linenos=false} 
# File/Directory To Publish
$(Build.ArtifactStagingDirectory)/$(Build.BuildId)-tfplan.tgz

# Artifact Name
$(Build.BuildId)-tfplan
```

#### Terraform Apply Release Pipeline

**Extract Archive**
```go {linenos=false} 
$(System.ArtifactsDirectory)/_Terraform Plan/$(Build.BuildId)-tfplan/$(Build.BuildId)-tfplan.tgz
```

**Terraform Init**
```go {linenos=false} 
terraform init -backend-config="access_key=$(NAME OF KEY VAULT SECRET FOR STORAGE ACCOUNT KEY)"
```

**Terraform Apply**
```go {linenos=false} 
terraform apply -auto-approve -input=false tfplan 
```

#### Terraform File Examples

**backend.tf**
```go {linenos=true} 
terraform {
  backend "azurerm" {
    storage_account_name = "tfazdodemostg001"
    container_name       = "terraform-state"
    key                  = "tf-azdo-demo.tfstate"
  }
}
```

**providers.tf**
```go {linenos=true} 
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "2.56.0"
    }
  }
}

provider "azurerm" {
  features {}
  subscription_id = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  client_id       = var.spn-client-id
  client_secret   = var.spn-client-secret
  tenant_id       = var.spn-tenant-id
}
```

**variables.tf**
```go {linenos=true} 
variable "spn-client-id" {}
variable "spn-client-secret" {}
variable "spn-tenant-id" {}
```