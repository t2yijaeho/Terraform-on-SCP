# 1. Install Terraform for SCP

To begin working with the Samsung Cloud Platform (SCP) using Terraform, you need to follow a few installation steps. Below are the instructions to set up the necessary components

## 1.1 SCP User Configuration

To configure the SCP user and credentials, perform the following steps

### 1.1.1 Create SCP Access Key

Refer to the ***[SCP API Guide](https://cloud.samsungsds.com/openapiguide/#/docs/v2-en-overview-overview)*** for instructions on creating an SCP access key

### 1.1.2 Configure SCP User and Credential

Create a directory for SCP configuration by executing the following command:

```Bash
mkdir -p ~/.scp
```

Create a configuration file by running the following command
>***Replace `<User Samsung Resource Name>`, `<User eMail Address>`, `<SCP Project ID>`***

```Bash
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

```Bash
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

```Bash
cat ~/.scp/config.json
```

The output should be similar to the following

```Bash
ubuntu@SCP:~$ cat ~/.scp/config.json
{
    "target": "production",
    "user-id": "srn::::PROJECT-xxxxxxx-xxxxxxxxxxxxxx:iam:user/xxxxx",
    "email": "scp.support@gmail.com",
    "project-id": "PROJECT-xxxxxxx-xxxxxxxxxxxxxx"
}
```

Next, execute the following command to view the contents of the credentials.json file

```Bash
cat ~/.scp/credentials.json
```

The output should be similar to the following

```Bash
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

```Bash
sudo apt update && sudo apt install -y gnupg software-properties-common
```

### 1.2.2 Install GPG Key

>[HashiCorp GPG key](https://www.hashicorp.com/security) for [apt](https://en.wikipedia.org/wiki/APT_(software)) package manager

To install the HashiCorp GPG key, run the following commands

```Bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

To verify the GPG key, execute the following command

```Bash
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
```

### 1.2.3 Add the Official HashiCorp Repository

Add the official HashiCorp repository to your system using the following command

```Bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list > /dev/null
```

### 1.2.4 Update and Install Terraform

>[APT Packages for Debian and Ubuntu](https://developer.hashicorp.com/terraform/cli/install/apt)

To download the package information from HashiCorp and install Terraform from the new repository, run the following command

```Bash
sudo apt update && sudo apt install -y terraform
```

### 1.2.5 Enable Terraform Auto-Completion

>[Install Terraform with Shell Autocompletion](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform-with-shell-autocompletion)

To enable auto-completion for Terraform in your shell, follow these steps

Ensure that the .bashrc file exists by executing the following command

```Bash
touch ~/.bashrc
```

Install the Terraform autocomplete plugin with the following command

```Bash
terraform -install-autocomplete
```

Restart the shell for the changes to take effect

```Bash
exec bash
```
