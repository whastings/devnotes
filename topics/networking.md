# Networking Notes

## Subnet

* A specific range of IP addresses
* Usually either public or private
  * Public ones have external and internal IPs
* In AWS, each instance must be assigned to a subnet

## NAT Gateway

* NAT = **Network Address Translation**
* Translates internal IPs into external IPs

## CIDR Blocks

* Notation for representing a range of IP address
* CIDR = **Classless Inter-Domain Routing**
* e.g. 192.0.2.0/24
  * Means IP range of 192.0.2.*
  * 24 = 24 bits
    * Covers first three numbers in the IP (because each number is 8 bits/one byte)
