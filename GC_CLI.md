# Instructions to Setup the Genesys Cloud CLI
Genesys CLI is a command-line tool avaiable for Windows, macOS, and Linux. This tool is a wrapped for the Genesys APIs. It provides the same information in an easier to use form for desktop use.

## Important Notes for OhMyZsh Users
OhMyZsh has an alias that will conflict with gc. To correct this, either add `unalias gc` to the end of your ~/.zshrc, or rename the gc binary to another suitable name. 

## Install Option 1 (This will pull the latest version directly from Git)
### Install and Setup Go 
Follow the instructions for your OS on [go.dev](https://go.dev/doc/install)<br>
Once Go is installed, refer to these [instructions](https://go.dev/doc/code#GOPATH) for adding GOPATH to your PATH variable. 

### Install the CLI
Run the command below from the terminal
```bash
env GO111MODULE=on go install github.com/mypurecloud/platform-client-sdk-cli/build/gc@latest
```

## Install Option 2 (OS Dependent)
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
Download from the Genesys [CDN](https://sdk-cdn.mypurecloud.com/external/go-cli/windows/latest/gc.exe). Once downloaded, move the executable to a directory on your PATH. For more information about the PATH variable on Windows, refer to [this page](https://superuser.com/questions/284342/what-are-path-and-other-environment-variables-and-how-can-i-set-or-use-them#:~:text=or%20user%20sessions.-,Path,-One%20of%20the).

## Setup
Before we set up the CLI, you'll need to create an OAuth Grant. Go to your instance and set up an OAuth client. You can refer to [this](https://help.mypurecloud.com/articles/create-an-oauth-client/) page for detailed instructions. I beleive Client Credentials is the Genesys recommended type based [this](https://www.youtube.com/watch?v=OnYDs5NsLpU&list=PL01cVFOkuN70Rk8xgI8pk_tKMcTW4FesF&index=2) devdrop video. Be sure to assign appropiate roles if using this grant type. Copy the client ID and client secret once you have the OAuth client setup.
In your terminal run the command below to create a new profile
``` bash
gc profiles new
```
You'll need to enter the following
- Name 
  - Used by you to keep multiple profiles straight
- Environment
  - What AWS region your organization is located in
  - Please refer to [this](https://help.mypurecloud.com/articles/change-the-region-of-your-genesys-cloud-organization/) page for a complete list of environments
- Access Token
  - Depending on the type of OAuth client you created, you may have an access token to enter. If not, just leave it blank.
- Client ID
  - This is the client ID we saved before when we created the OAuth Client
- Client Secret
  - This is the client secret we saved before when we created the OAuth Client.

# Basic Usage Examples
### Notes
Before we jump into the examples, I'd like to cover a few basic features of command-line programs. Typically, CLI programs come with documentation built-in. Depending on your OS, this can be accessed in a few ways. The first way is using the `-h` or `--help` arguments. `gc -h` / `gc --help` would return the help information for the GC executable. Often, this can be used to get more information about specific arguments as well. `gc queues -h` would return the help information for the queues argument. The second way is using the `man` executable to access the program’s manual pages. `man gc` would return the manual pages for the Genesys Cloud CLI. <br>
You'll typically use arguments when running CLI programs. These arguments can either be single letters or full words depending on the program and argument. When using full words, you'll need two dashes preceding the argument; for example, `gc --help.` When using single letters, you'll need one dash preceding the argument, for example, `gc -h.` When using multiple single-letter arguments, you can either group them all behind one dash `ls -lsa` or you can separate them `ls -l -s -a`. Both options would return the same result

### Video Example
[Here](https://www.youtube.com/watch?v=OnYDs5NsLpU&list=PL01cVFOkuN70Rk8xgI8pk_tKMcTW4FesF) is a brief video demonstration made by Genesys. 

### Basic Examples
For the following examples, you must have the Genesys Cloud CLI and jq installed. These examples use bash/zsh but can be modified for PoweerShell.

#### Get a list of all avaible queues
```bash
gc routing queues list
```
The above command will return the entire JSON result. It can be a bit overwhelming. You can also use a program like jq to parse the output and only return the information you want. You'll likely want a jq cheat sheet to refer to. Olih makes the one I'm currently using. Here is the [link](https://gist.github.com/olih/f7437fb6962fb3ee9fe95bda8d2c8fa4). This can also be a place to use an AI tool like ChatGPT. You'll want to ensure you aren't passing any confidential data when using an AI tool. The best way to avoid this is by running the command to get the output you want to parse and remove all but like 3 or 4 results while ensuring you keep the JSON structure. Then, replace any confidential information with fake data. In the command above, there isn't much that could be considered confidential, but for other commands, you can replace phone numbers with "+15555555555", replace names with "John Doe", etc. Then, include that sanitized output in your prompt. Anytime you use an AI tool, you'll need to ensure the data you give is ok to publicly share, ask yourself "Would I post this online?" and the result is correct. Always test the results before assuming they are correct.

#### Get a list of queue names
```bash
gc routing queues list | jq '.entities[].name'
```

#### Get a list of queue names with IDs
```bash
gc routing queues list | jq '.entities[] | {name, id}'
```

#### Get a list of the agent names and IDs assigned to a specific queue
```bash
QUEUE_NAME="REPLACE_WITH_THE_QUEUE_YOU_WANT_TO_USE" \
QUEUE_ID=$(gc routing queues list | jq -r --arg queue_name "$QUEUE_NAME" '.entities[] | select(.name == $queue_name) | .id') \
gc routing queues members list "$QUEUE_ID" | jq '.entities[] | {agent_name: .name, agent_id: .id}'
```