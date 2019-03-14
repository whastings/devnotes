# Terraform

* Tool/programming language for setting up cloud resources
  * Can do AWS and others
* Is a declarative language for saying what state you want your resources to look like
  * It keeps track of what needs to be updated

## Resource

* Something Terraform can create, manage, and destroy
* e.g. Instance, load balancer, security group
* Each has a type, name, and zero or more parameters
* Resource definitions can refer to variables that come from other resources definitions
* Form a DAG representing the relationships between resources
  * e.g. You need to create a volume before you create an attachment

## Module

* A reusable group of resources
* Kind of like a function
* Can take parameters you can use to fill in details of resources
* A module can reference another module and reuse its resource definitions

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
