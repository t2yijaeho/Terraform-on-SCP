# 2. Terraform Provider for SCP

>Terraform relies on plugins called providers to ***interact with cloud providers, SaaS providers, and other APIs***
>Terraform configurations must ***declare which providers they require*** so that Terraform can install and use them. Additionally, some providers require configuration (like endpoint URLs or cloud regions) before they can be used

## 2.1 [Terraform Registry](https://registry.terraform.io/browse/providers)

>main directory of publicly available Terraform providers

### 2.1.1 Terraform Block

To use a provider from the Terraform Registry, you need to add it to your Terraform configuration. The following code block shows how to add the Samsung Cloud Platform (SCP) provider to your configuration

```ruby
terraform {
  required_providers {
    scp = {
      version = "1.8.5"
      source  = "SamsungSDSCloud/samsungcloudplatform"
    }
  }
}
```

The ***required_providers*** block specifies the providers that your Terraform configuration depends on. In this case, we are specifying that our configuration depends on the SCP provider. The version attribute specifies the version of the SCP provider that we want to use. The source attribute specifies the source of the SCP provider. In this case, the SCP provider is hosted on GitHub by the SamsungSDSCloud organization.

Once you have added the SCP provider to your Terraform configuration, you can use it to manage SCP resources. For more information on how to use the SCP provider, please refer to the [Samsung Cloud Platform Provider Documentation in Terraform Registry](https://registry.terraform.io/providers/SamsungSDSCloud/samsungcloudplatform/latest/docs)

### 2.1.2 Provider Requirement

***local name (scp)*** unique identifier used everywhere else in a Terraform module

***source (SamsungSDSCloud/samsungcloudplatform)*** the global source address for the provider you intend to use

***version (1.8.5)*** a version constraint specifying which subset of available provider versions the module is compatible with

[Version Constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)

  - "1.8.5": Allows only one exact version number
  - "~> 1.8.5": Allows only the rightmost version component to increment. allow installation of 1.8.7 and 1.8.10 but not 1.9.0
  - "=> 1.8.5": Comparisons against a specified version, allowing versions for which the comparison is true. "Greater-than" requests newer versions

