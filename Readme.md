# Deploy an network account with AFT

network.tf consist of code provisioning of network account which will consist of core infrastructure resorces required for setting up Transit Gateway and IPAM pool which would be shared with PROD OU and DEV OU using Resources Access Manager(RAM)

## Scope
network.tf would be responsible for provisioning the network account with SSO and also executing the terraform script placed in aft-account-customizations repository under terraform folder with naming convention of folder as "network" which is responsible for IPAM pool creation and Transit Gateways one for each environment(Dev and Prod).

## Usage
```
module "network" { 
  source = "./modules/aft-account-request"

  control_tower_parameters = {
    AccountEmail              = "rob+network@example.com" 
    AccountName               = "network" 
    ManagedOrganizationalUnit = "SharedOU (ou-d55u-2fi7cs47)" 
    SSOUserEmail              = "rob+network@example.com" 
    SSOUserFirstName          = "rob" 
    SSOUserLastName           = "martin" 
  }

  account_tags = {
    "Learn Tutorial" = "AFT"      
  }

  change_management_parameters = {     
    change_requested_by = "HashiCorp Learn"
    change_reason       = "Learn AWS Control Tower Account Factory for Terraform"
  }

  custom_fields = { 
    prd_ipam_cidr = "10.101.0.0/16"  
    dev_ipam_cidr = "10.100.0.0/16"  
  }
  
  account_customizations_name = "network" 
}

```
#### The code above will provision the following:
```
✅ network account under Organization Unit SharedOU
✅ Provison SSM parameter for setting IPAM pool CIDR as parameter using custom fields which will be used for account customization for setting IPAM pool.
✅ Attribute Account customization with value "network" will execute code within aft-account-customizations repository required for provisioning IPAM Pool and Tranist Gateways
```
### Attributes in Code
```
✅ The account_tags attribute lets you apply tags to your account according to your business criteria
✅ The change_management_parameters attribute lets you document who issues the account request and its purposes
✅ The custom_fields attribute lets you define additional metadata for your account, which you can use in your account customizations or provisioning configuration
✅ The account_customizations_name attribute lets you specify the subdirectory in the account customizations repository the pipeline should use to modify this account, if any
```
#### Custom Fields

Using Custom Fields we will be setting up IPAM CIDR ranges for IPAM Pools for each respective environment like DEV and PROD.

## Requirements

| Name          | Version       | 
| ------------- | ------------- |
| Terraform     | >= 1.0.0      | 
| AWS           | >= 3.72       | 

## Providers

| Name          | Version       | 
| ------------- | ------------- | 
| AWS           | >= 3.72       |

## Output 

This will create network account within your specfied OU and you can check the account created in AWS Organization as well as you will get notification on your provided AccountEmail Id.










