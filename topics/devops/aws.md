# Amazon Web Services

## Security Group

* A virtual firewall that controls network traffic to and from an instance
* Controls which ports, protocols, and IP ranges an instance can receive requests from
  * Can use `0.0.0.0/0` to allow requests from/to anywhere
* Can also list other security groups instead of IP ranges for where requests are allowed from
  * And then you can assign those groups to other instances so those instance are allowed to communicate with your security group

## Elastic Compute Cloud (EC2)

* Provides host *instances* as virtual machines running on a *hypervisor*
* Can have either:
  * Shared tenancy: Your instances share same machine as other customers'
  * Dedicated tenancy: Your instances get their own machine
* Instances only have temporary hard drive storage
  * Goes away when they're rebooted
* **Amazon Machine Image** (AMI): Starting image for creating an instance
  * Are different kinds for different OSes

## Virtual Private Cloud (VPC)

* Your own software-defined network
* Instances on the same VPC can talk to each other
  * But ones in different VPCs can't
* Good for isolating data and services for security
  * e.g. One VPC for Staging and one for Production

## Elastic Block Store (EBS)

* Service for persistent file storage
* Like virtual hard drives
* Each can be attached to one instance at a time
* You have to pick a file system for it
  * Then the FS is mounted to the instance for read/write

## Load Balancers

* Two kinds
  * ALB
  * ELB
* AWS automatically handles horizontal scaling of load balancer instances

## Route 53

* Amazon's DNS system/service

## Elastic IP

* Service for reserving dedicated IPs for your instances

## Availability Zones

* Usually maps to one physical data center
* Can be multiple per region
* You can distribute your hosts amongst them in case one of them goes down

## Relational Database Service (RDS)

* Amazon-hosted and managed database server
* Supports Postgres, MySQL, etc.
* Has advanced features like:
  * Automated backups
  * Point-in-time restores
  * Master and replica hosts for failovers
    * If master fails, a replica can become the new master

## IAM Roles

* IAM = Identity and Access Management
* **Users**
  * Can be users who can log into AWS console or users who can make AWS API calls
  * For making API calls, have a key and secret for signing requests to Amazon
* **Policies**
  * Defines a group of actions on resources
  * Can be applied to a Role or User
  * Can test with [IAM Policy Simulator](https://policysim.aws.amazon.com/home/index.jsp)
* **Roles**
  * Can have one or more policies assigned to them
  * Resources can *assume* roles in order to get access to their policies
    * Instances can assume roles in order to make AWS API calls
    * People can be granted roles when they log in through something like Okta
