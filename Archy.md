# Instructions to install and setup Archy
Archy is a Genesys Cloud Architect YAML processor that lets you create Architect flows from YAML files. You can also export existing flows for use with Archy.


## Prerequisites
Before you install and Run Archy, you'll need to ensure you have an OAuth Client setup. Refer to [this](https://help.mypurecloud.com/articles/create-an-oauth-client/) page for instructions on setting this up. You'll want to use the following settings 
- Token duration -> `86400 seconds`
- Grant Type -> `Client Credentials`
- Roles -> `Add the roles required for your use case. Ensure the client is assigned the architect:ui:view permission as that is required to use architect scripting which is part of archy`
Once you've created the client, note the `Client ID` and `Client Secret` for use later.


## Installation
Refer to the [Genesys Documentation][this](https://developer.genesys.cloud/devapps/archy/install#installation) for installation instructions

## Archy Setup
Once you have the OAuth Client setup and Archy installed, open a terminal and enter `archy setup`<br>
Archy will run through its setup process
- Confirm you've completed the steps above
- Select a home directory
    - This directory will store the results of some commands as well as debug information.
- Select a location.
    - This is the region your Genesys environment is setup in. Refer to [this](https://help.mypurecloud.com/articles/change-the-region-of-your-genesys-cloud-organization/) page for a list of possible options.
- Select if you want debug information enabled
- Enter your Client ID
- Enter your Client Secret
- Enter your Auth Token, if you have one
- Perform a refresh

## Archy usage examples.
### Export a flow
```bash
archy export --flowName "main" --flowType inboundcall --exportType yaml --outputDir output --force;
```

### Export all flows (Example copied from [Genesys](https://developer.genesys.cloud/blog/2021-11-06-exporting-archy-flows-in-bulk/))
For a detailed explaination of these steps refer to the link in the header. 
To accomplish this, we'll need to use the transform feature of the Genesys CLI. First we need to write the transform. Copy the transform below into a file called `archy_export_all.tmpl`. 
```bash
{{- range . -}}{{printf "archy export --flowName \"%s\" --flowType %s --exportType yaml --outputDir output --force\n" .name (lower .type)}}{{end}}`
```
Then run the following command, be sure to update the transform file location (archy_export_all.tmpl) and the Output file location (export_architect_flows.sh) as needed.
```bash
gc flows list -a --transform archy_export_all.tmpl > export_architect_flows.sh`
```
This will produce a file called export_architect_flows.sh that will contain the archy export command for each flow in your environment.
``` bash
...
archy export --flowName "Default In-Queue Flow" --flowType inqueuecall --exportType yaml --outputDir output --force
archy export --flowName "main_emergency" --flowType inboundcall --exportType yaml --outputDir output --force
archy export --flowName "main_closed" --flowType inboundcall --exportType yaml --outputDir output --force
...
```

### Archy with Terraform
ToDo (Will mostly be housed in the IaC repo, but I'll pull a couple examples here.)