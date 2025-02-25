# Genesys Guides
This is my attempt to localize information related to the different Genesys Tools. This repo will include setup, basic usage, and information on how these tools work together. This is not intended to be a replacement for existing documentation. <br>

## API Explorer
API Explorer allows you to view documentation and SDK usage and make requests to the Genesys Cloud Public API from your browser. <br>
[Genesys Documentation](https://developer.genesys.cloud/devapps/about/api-explorer)<br>
[API Explorer](https://developer.genesys.cloud/devapps/api-explorer)<br>

## Genesys Cloud CLI
Genesys CLI is a command-line tool available for Windows, macOS, and Linux. This tool is a wrapper for the Genesys APIs. It provides the same information in an easier to use form for desktop use. <br>
[Genesys Documentation](https://developer.genesys.cloud/devapps/cli/)<br>
[My GC ClI Guide](GC_CLI.md)<br>

## Archy
Archy is a Genesys Cloud Architect YAML processor that lets you create Architect flows from YAML files. You can also export existing flows for use with Archy.<br>
[Genesys Documentation](https://developer.genesys.cloud/devapps/archy/)<br>
[My Archy Guide](Archy.md)<br>

## Terraform
Terraform is an Infrastructure as Code (IaC) tool that allows you to define and manage your cloud resources using declarative configuration files. Using the Genesys Provider, you can provision, configure, and manage your Genesys Environment using these configuration files.<br>
[Terraform Documentation](https://developer.hashicorp.com/terraform/intro)<br>
[Genesys Documentation](https://developer.genesys.cloud/devapps/cx-as-code/)<br>
[Provider Documentation](https://registry.terraform.io/providers/MyPureCloud/genesyscloud/latest/docs)<br>
[My Terraform Guide](terraform.md)<br>
[My Infrastructure as Code Guide](../IaC/README.md)<br>

## SDKs
Genesys Cloud SDKs allow programmatic access to the Genesys APIs, allowing for automated processes and integration with third-party systems. They have SDKs for .NET, iOS, Go, Python, JavaScript, and Java.<br>
~~My SDK Guide~~ ToDo<br>
[Genesys Documentation](https://developer.genesys.cloud/devapps/sdk/docexplorer)<br>

# Other Tools
Here are a few additional tools that can enhance the functionality and usability of Genesys tools.<br>

## jq
jq is a CLI JSON processor. It allows you to parse, filter, and transform JSON. We will use it to manipulate the results from the CLI. 

### Installing jq
Refer to the [downloads](https://jqlang.org/download/) page for your OS.<br>