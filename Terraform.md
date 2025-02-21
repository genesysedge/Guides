# Instructions to Setup Terraform for Genesys Cloud
Terraform is an Infrastructure as Code (IaC) tool that allows you to define and manage your cloud resources using declarative configuration files. Using the Genesys Provider, you can provision, configure, and manage your Genesys Environment using these configuration files.<br>
Genesys Refers to their Terraform implementation as CX as Code instead of infrastructure as Code, so we will be referring to it as CX as Code moving forward.

## Installation
Refer to [this](https://developer.hashicorp.com/terraform/install) page for Terraform installation files and instructions<br>

## Setup and Use
You'll need an OAuth Client setup with appropiate permissions for use with Terraform. The permissions required will vary by use case so a single list doesn't exist. You can refer to the [Genesys Documentation](https://help.mypurecloud.com/articles/create-an-oauth-client/) for information on that process.
Refer to the [IaC repo](../IaC/README.md) for further information. 