# Terraform

## Commands

* `terraform apply`: Run your configs against your provider
* `terraform show`: Show info about current setup on your provider
* `terraform destroy`: Destroy setup on your provider

## Provisioners

* Only run when a resource is created
  * Just for use in bootstrapping a server
* You can also create Destroy Provisioners that run when a resource is destroyed

## Variables

* Let you parameterize your config
* Setting variables
* Required variable syntax: `variable "name" {}`
* Variable with default value syntax:
```
variable "name" {
  default = "value"
}
```
* Referencing a variable: `${var.name}`
* Can be set via a `*.tfvars` file
* List value syntax: `[val1, val2]`
* Map value syntax: `{ "key" = value }`
  * Referencing a map value: `${lookup(var.mapName, nameOfKey)}`

## Outputs

* Values you want Terraform to output about your setup
* Syntax: `output "name" { value = "value" }`
* Value can contain interpolations
* Look up with `terraform output name`
