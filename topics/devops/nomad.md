# Nomad Notes

* System for running container-based services
  * Has a pool of machines it can run stuff on
    * And it divides work between them
* Handles creating or updating hosts
* Handles creating new processes on hosts
  * e.g. If you increase the count
* Handles *migrating* a service from one host to another
* Handles running health check on process to make sure it started successfully
* Can limit process memory usage to a set amount
* Can run processes in Docker containers
  * There's a `docker` task driver
  * Handles creating public host ports and linking them to ports of docker containers
* Connects with Consul for service discovery
  * Registers each service it manages with Consul
* Also supports setting environment variables
* But doesn't solve request routing for you

## Job Specs

* Tell Nomad what state you want your hosts to be in
* Each Job has a set of *Groups*

## Groups

* Each one defines a related set of *Tasks*
* Has a `count` to determine how many *allocations* of that group there will be
  * All tasks in an allocation run on the same machine and have access to the same filesystem

## Tasks

* Task defines a process/service to run on a host
  * e.g. Web app server, background job server, etc.
* Its config takes command to run and arguments to pass to it
  * `docker` driver supports running the command in docker containers
* Can specify amounts of CPU, memory, and networking resources that the process should have
* Can define environment variables available to the process
* Can define details for running health check on process after start

## Levant

* Go-based templating language/tool used to preprocess job specs
* Supports conditionals and loops
* Supports pulling in variables from YAML config files
  * e.g. So you can have one config file per environment (staging, production, etc.)
  * For things like process counts, memory limits, etc.
