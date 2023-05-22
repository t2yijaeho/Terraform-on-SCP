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

### 1.1.3 Verify SCP IAM Configuration

```Bash
cat ~/.scp/config.json
```

```Bash
ubuntu@SCP:~$ cat ~/.scp/config.json
{
    "target": "production",
    "user-id": "srn::::PROJECT-xxxxxxx-xxxxxxxxxxxxxx:iam:user/xxxxx",
    "email": "scp.support@gmail.com",
    "project-id": "PROJECT-xxxxxxx-xxxxxxxxxxxxxx"
}
```

```Bash
cat ~/.scp/credentials.json
```

```Bash
ubuntu@SCP:~$ cat ~/.scp/credentials.json
{
    "auth-method": "access-key",
    "access-key": "xxxxxxxxxxxxxxxx",
    "secret-key": "xxxxxxxxxxxxxxxxxxxxxxxxx"
}
```

## 1.2 Install Terraform

> [Install Terraform on Linux (Ubuntu/Debian)](https://developer.hashicorp.com/terraform/downloads)

### 1.2.1 Ensure gnupg, software-properties-common packages installed

- Use these packages to verify HashiCorp's GPG signature and install HashiCorp's Debian package repository

```Bash
sudo apt update && sudo apt install -y gnupg software-properties-common
```

### 1.2.2 Install GPG Key

>[HashiCorp GPG key](https://www.hashicorp.com/security) for [apt](https://en.wikipedia.org/wiki/APT_(software)) package manager

- Install the HashiCorp GPG key

```Bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

- Verify the GPG key

```Bash
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
```

### 1.2.3 Add the official HashiCorp repository

```Bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list > /dev/null
```

### 1.2.4 Update and install Terraform

>[APT Packages for Debian and Ubuntu](https://developer.hashicorp.com/terraform/cli/install/apt)

- Download the package information from HashiCorp and install Terraform from the new repository

```Bash
sudo apt update && sudo apt install -y terraform
```

### 1.2.5 Terraform auto completion

>[Install Terraform with Shell Autocompletion](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform-with-shell-autocompletion)

- Ensure that a config file exists

```Bash
touch ~/.bashrc
```

- Install the autocomplete plugin

```Bash
terraform -install-autocomplete
```

- Restart the shell

```Bash
exec bash
```
