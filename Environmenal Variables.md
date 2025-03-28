# How to Set Up Environment Variables in Different Tools

This is my solution for setting up environment variables to be used with Genesys tools. I struggled for quite a while, so hopefully, this will save someone else some time. I wasn't able to get the CLI variables set up correctly, so please submit a pull request if you know what needs to change.

## Enviroment Setup

Open the rc file for whatever shell you are using. For me, this was `~/.zshrc`.

Now, scroll to the bottom and add the following. You can find the region mapping [here](https://help.mypurecloud.com/articles/change-the-region-of-your-genesys-cloud-organization).

```bash
# Genesys Cloud 
## Exports
export GENESYSCLOUD_REGION="REPLACE_WITH_YOUR_REGION"
export GENESYSCLOUD_OAUTHCLIENT_ID="REPLACE_WITH_YOUR_OAUTH_CLIENT_ID"
export GENESYSCLOUD_OAUTHCLIENT_SECRET="REPLACE_WITH_YOUR_OAUTH_CLIENT_SECRET"

### Remaps the above exports to their Terraform equivalents. 
export TF_VAR_oauthclient_id=$GENESYSCLOUD_OAUTHCLIENT_ID
export TF_VAR_oauthclient_secret=$GENESYSCLOUD_OAUTHCLIENT_SECRET
export TF_VAR_aws_region="REPLACE_WITH_YOUR_AWS_REGION"  # Please note: This is different from the Genesys Cloud region and will be something like us-west-1, us-east-2, etc.
```

I also had to add the following function since I wasn't able to get the variable mapping to work with the Genesys CLI config file. I'll explain this more in the Genesys CLI section.
```bash
gcp() {
    gc --environment $GENESYSCLOUD_REGION --clientid $GENESYSCLOUD_OAUTHCLIENT_ID --clientsecret $GENESYSCLOUD_OAUTHCLIENT_SECRET "$@"
}
```
Now that we have the variables set up, you'll want to source the rc file. Change the file name to match your shell.
```bash
source ~/.zshrc
```
## Terraform Configuration

Ensure your files look like mine.

`provider.tf` (Or whatever file contains your provider information)
```terraform
provider "genesyscloud" {
  oauthclient_id     = var.oauthclient_id
  oauthclient_secret = var.oauthclient_secret
  aws_region         = var.aws_region
}
```

`Variables.tf`
```terraform
variable "aws_region" {
  description = "AWS Region for the Genesys Cloud Instance"
  type        = string
}

variable "oauthclient_id" {
  description = "OAuth Client ID we are using to apply the Terraform configuration"
  type        = string
}

variable "oauthclient_secret" {
  description = "OAuth Client Secret we are using to apply the Terraform configuration"
  type        = string
}
```

If you have a `.tfvars` file, make sure it does not contain any mappings for these variables.

Once the above is in place, open a terminal, cd into your terraform directory and run `terraform plan`

Terraform should successfully complete the plan while using the enviromental variables. If you would rather store the variables in a file, you can create a .env file in your Terraform directory that holds the samme exports we put into .zshrc. To use this file you'll run `source .env`
Once you've sourced the env file, proceed with running Terraform.

## CLI/SDK Configuration

I wasn't able to get this working via a profile, so I'm using overrides in the command itself. I created the `gcp` function above to make it easier to run. From the [documentation](https://github.com/MyPureCloud/platform-client-sdk-cli#environment-variables:~:text=on%20the%20CLI.-,Environment%20variables,-The%20following%20environment) I assume you could make this work by creating a profile that uses the variables, but I wasn't able to. 