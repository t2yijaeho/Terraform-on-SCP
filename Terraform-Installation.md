# 1. Install Terraform for SCP

To begin working with the Samsung Cloud Platform (SCP) using Terraform, you need to follow a few installation steps. Below are the instructions to set up the necessary components

## 1.1 SCP User Configuration

To configure the SCP user and credentials, perform the following steps

### 1.1.1 Create SCP Access Key

Refer to the ***[SCP API Guide](https://cloud.samsungsds.com/openapiguide/#/docs/v2-en-overview-overview)*** for instructions on creating an SCP access key

### 1.1.2 Configure SCP User and Credential

Create a directory for SCP configuration by executing the following command:

```sh
mkdir -p ~/.scp
```

Create a configuration file by running the following command
>***Replace `<User Samsung Resource Name>`, `<User eMail Address>`, `<SCP Project ID>`***

```sh
cat <<EOF >~/.scp/config.json
{
    "target": "production",
    "user-id": "<User Samsung Resource Name>",
    "email": "<User eMail Address>",
    "project-id": "<SCP Project ID>"
}
EOF
```

Create a credential file with the following command
>***Replace `<Access Key>`, `<Secret key>`***

```sh
cat <<EOF >~/.scp/credentials.json
{
    "auth-method": "access-key",
    "access-key": "<Access Key>",
    "secret-key": "<Secret key>"
}
EOF
```

### 1.1.3 Verification of SCP IAM Configuration

To verify the configuration of SCP IAM (Secure Cloud Protocol Identity and Access Management) settings, follow these steps

Execute the following command to view the contents of the config.json file

```sh
cat ~/.scp/config.json
```

The output should be similar to the following

```sh
ubuntu@SCP:~$ cat ~/.scp/config.json
{
    "target": "production",
    "user-id": "srn::::PROJECT-xxxxxxx-xxxxxxxxxxxxxx:iam:user/xxxxx",
    "email": "scp.support@gmail.com",
    "project-id": "PROJECT-xxxxxxx-xxxxxxxxxxxxxx"
}
```

Next, execute the following command to view the contents of the credentials.json file

```sh
cat ~/.scp/credentials.json
```

The output should be similar to the following

```sh
ubuntu@SCP:~$ cat ~/.scp/credentials.json
{
    "auth-method": "access-key",
    "access-key": "xxxxxxxxxxxxxxxx",
    "secret-key": "xxxxxxxxxxxxxxxxxxxxxxxxx"
}
```

## 1.2 Installation of Terraform

> [Install Terraform on Linux (Ubuntu/Debian)](https://developer.hashicorp.com/terraform/downloads)

To install Terraform on your Linux system, please follow the steps below:

### 1.2.1 Verify Installation Prerequisites

Ensure that the `gnupg` and `software-properties-common` packages are installed on your system. These packages are required for verifying HashiCorp's GPG signature and installing HashiCorp's Debian package repository. Use the following command to install them

```sh
sudo apt update && sudo apt install -y gnupg software-properties-common
```

```sh
ubuntu@SCP:~$ sudo apt update && sudo apt install -y gnupg software-properties-common
Hit:1 https://cli.github.com/packages stable InRelease
.
.
.
software-properties-common set to manually installed.
0 upgraded, 0 newly installed, 0 to remove and 5 not upgraded.
```

### 1.2.2 Install GPG Key

>[HashiCorp GPG key](https://www.hashicorp.com/security) for [apt](https://en.wikipedia.org/wiki/APT_(software)) package manager

To install the HashiCorp GPG key, run the following commands

```sh
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

```sh
ubuntu@SCP:~$ wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
--2023-06-14 15:36:48--  https://apt.releases.hashicorp.com/gpg
Resolving apt.releases.hashicorp.com (apt.releases.hashicorp.com)... 54.230.167.111, 54.230.167.99, 54.230.167.46, ...
Connecting to apt.releases.hashicorp.com (apt.releases.hashicorp.com)|54.230.167.111|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3980 (3.9K) [binary/octet-stream]
Saving to: ‘STDOUT’

-                                     100%[======================================================================>]   3.89K  --.-KB/s    in 0s      

2023-06-14 15:36:48 (306 MB/s) - written to stdout [3980/3980]
```

To verify the GPG key, execute the following command

```sh
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
```

```sh
ubuntu@SCP:~$ gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
gpg: directory '/home/ubuntu/.gnupg' created
gpg: /home/ubuntu/.gnupg/trustdb.gpg: trustdb created
/usr/share/keyrings/hashicorp-archive-keyring.gpg
-------------------------------------------------
pub   rsa4096 2023-01-10 [SC] [expires: 2028-01-09]
      798A EC65 4E5C 1542 8C8E  42EE AA16 FCBC A621 E701
uid           [ unknown] HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
sub   rsa4096 2023-01-10 [S] [expires: 2028-01-09]
```

### 1.2.3 Add the Official HashiCorp Repository

Add the official HashiCorp repository to your system using the following command

```sh
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list > /dev/null
```

### 1.2.4 Update and Install Terraform

>[APT Packages for Debian and Ubuntu](https://developer.hashicorp.com/terraform/cli/install/apt)

To download the package information from HashiCorp and install Terraform from the new repository, run the following command

```sh
sudo apt update && sudo apt install -y terraform
```

```sh
ubuntu@SCP:~$ sudo apt update && sudo apt install -y terraform
Hit:1 https://cli.github.com/packages stable InRelease
.
.
.
The following NEW packages will be installed:
  terraform
0 upgraded, 1 newly installed, 0 to remove and 5 not upgraded.
Need to get 21.7 MB of archives.
After this operation, 65.0 MB of additional disk space will be used.
Get:1 https://apt.releases.hashicorp.com lunar/main amd64 terraform amd64 1.5.0-1 [21.7 MB]
Fetched 21.7 MB in 1s (36.5 MB/s)    
Selecting previously unselected package terraform.
(Reading database ... 40135 files and directories currently installed.)
Preparing to unpack .../terraform_1.5.0-1_amd64.deb ...
Unpacking terraform (1.5.0-1) ...
Setting up terraform (1.5.0-1) ...
```

### 1.2.5 Enable Terraform Auto-Completion

>[Install Terraform with Shell Autocompletion](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform-with-shell-autocompletion)

To enable auto-completion for Terraform in your shell, follow these steps

Ensure that the .bashrc file exists by executing the following command

```sh
touch ~/.bashrc
```

Install the Terraform autocomplete plugin with the following command

```sh
terraform -install-autocomplete
```

Restart the shell for the changes to take effect

```sh
exec bash
```

### 1.2.6 Verify the installation

Verify the installation by display the version of Terraform, the platform it's installed on, installed providers, and the results of upgrade and security checks

```sh
terraform version
```

```sh
ubuntu@T3XBC:~$ terraform version 
Terraform v1.5.0
on linux_amd64
```

```sh
ubuntu@T3XBC:~$ terraform version -json
{
  "terraform_version": "1.5.0",
  "platform": "linux_amd64",
  "provider_selections": {},
  "terraform_outdated": false
}
```
