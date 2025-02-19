# Instructions to Setup the Genesys Cloud CLI
## Prerequisites
> Please Note: ohmyzsh has an alias that will conflict with gc. To correct this either add unalias gc to the end of your ~/.zshrc, or rename the gc binary to another suitable name. 

## Install Option 1 (This will pull the lastest version directly from Git)
### Install and setup Go 
Follow the instructions for your OS on [go.dev](https://go.dev/doc/install)
Once Go is installed, refer to these [instructions](https://go.dev/doc/code#GOPATH) for adding GOPATH to your PATH variable. 

### Install the CLI
Run the command below from the terminal
```bash
env GO111MODULE=on go install github.com/mypurecloud/platform-client-sdk-cli/build/gc@latest
```
## Install Option 2 (OS Dependant)
Refer to the section below for your OS
### macOS
The most convenient method is to use [HomeBrew](https://brew.sh/). With Brew installed run the commands below
``` bash
brew tap mypurecloud/gc
brew install gc
```

### Linux
Run the following command from your terminal.
```bash
curl -s https://sdk-cdn.mypurecloud.com/external/go-cli/linux/dl/install.sh | sudo bash
```

### Windows
Download from the Genesys [CDN](https://sdk-cdn.mypurecloud.com/external/go-cli/windows/latest/gc.exe). Once downloaded, move the executable to a directory on your PATH. Fore more information about the PATH variable on Windows, refer to [this page](https://superuser.com/questions/284342/what-are-path-and-other-environment-variables-and-how-can-i-set-or-use-them#:~:text=or%20user%20sessions.-,Path,-One%20of%20the).


## Setup
Before we setup the CLI, you'll need to create an OAuth Grant. Go to your instance and setup an OAuth client. I used a Client Credentials grant since it was easy to setup and I'm doing this with a lab instance. Once you have the OAuth client setup, be sure to copy the client ID and client secret.
In your terminal run the command below to create a new profile
``` bash
gc profiles new
```

You'll need to enter the following
- name 
  - Used by you to keep multiple profiles straight
- Enviroment
  - What AWS region your organization is located in
  - Please refer to [this](https://help.mypurecloud.com/articles/change-the-region-of-your-genesys-cloud-organization/) page for a complete list of enviroments
- Access Token
  - Depending on the type of OAuth client you created, you may have an access token to enter. If not, just leave it blank.
- Client ID
  - This is the client ID we saved before when we created the OAuth Client
- Client Secret
  - This is the client secret we saved before when we created the OAuth Client.
