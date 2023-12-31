# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioing for its tagging.
[semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, e.g. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes



## Install the Terraform CLI

### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest install CLI instructions via Terraorm Documentation and change the scripitng for install. 

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distrinution
This project is built against Ubuntu distribution. Please check accordingly to your distribution needs.
[How to check OS version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of checking OS version
```
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```


### Refactoring insto Bash Scripts 

While fixing the Terraform CLI gpg depreciation issues, we notice that the bash scripts steps were a considerable amount more code. So we decided to create a bash script to install the Terraform CLI.

This is the new bash script located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml)) tidy.
- This will allow us to easily debug and execute manually Terraform CLI install
- This will allow better portability for other projects that need to install Terraform CLI.

### Shebang Considerations

A shebanng (pronounced sha-bang) tells the bash script what program that will interpret the script. e.g. `#!/bin/bash`

ChatGPT recommends this format for bash: `#!/usr/bin/env bash`
- For portability for different OS distributions
- Will search the user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_(Unix)

### Execution Considerations
When executing the bash script we can use the `./` shorthand notiation to execute the bash

e.g. `./bin/install_terraform_cli`

if we are using a script in .gitpod.yml we need to point the script to a program to interpret it.

e.g. `source ./bin/install_terraform_cli`

### Linux Considerations

In order to make our bash script executable, we need to change linux permissions for the file to be executable at the user level.

```sh
chmod u+x ./bin/install_terraform_cli
```
Alternatively:

```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/Chmod

> Having a bin folder and putting all your script without an extension is the common normal way.


### Gitpod Lifecycle (Before, Init, Command)

We need to be careful when using the init because it will not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks

### Working with ENV Vars

### env command

We can list out all Environment Variables (Env Vars) using the `env` command

We can filter specific env vars using grep e.g. `env | grep AWS_`

### Setting and Unsetting Env Vars

In the termainal we can set using `export HELLO='world`

In the terminal we unset using `unset HELLO`

We can set an env var temporarily when just running a command

```sh
HELLO='world' ./bin/print_message
```

Within a bash script, we can set an env without writting export e.g.

```sh
#!/usr/bin/env bash

HELLO='world'

echo $HELLO
```
### Printing Vars

We can print an env var using echo e.g. `echo $HELLO`


#### Scoping of Env Vars

When you open up a new bash terminal in VScode it will not be aware of env vars that you have set in another window.

If you want to Env Vars to persist across all future terminals that are open, you need to set env vars in your bash profile. e.g. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can presist env vars into gitpod by storing them in Gitpod secrets storage.

```
gp env HELLO='world'
```

All future workspaces lauched will set the env vars for all bash terminals opened in those workspaces.


You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation

AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started: Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)


[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our aws credentials is configured correctly by running the following AWS CLI command: 

```sh
aws sts get-caller-identity
```

if it is succesful you should see a json payload returnn that looks like this:

```json
{
    "UserId": "AIDA25FJ43RYJZEXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/terraform-beginner-bootcamp"
}
```

> We'll need to generate AWS CLI credentials from IAM USER in order to use AWS CLI.