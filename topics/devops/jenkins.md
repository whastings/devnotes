# Jenkins

* Started as a Java CI project called Hudson

## Job

* Config can come from Jenkins UI or `Jenkinsfile`
* Could be CI, important script, backups
* Can be run via various triggers
  * e.g. Manual, git push, schedule, after another job completes

## Hosts/instances

* There is a *master* host and *worker* hosts
* Best not to run jobs on master
  * Master has to be created manually or by Terraform
* Each host can have one or more **Executors**
  * Docker makes it easier to run multiple because the state would be independent
* Connecting to workers
  * Jenkins connects to worker hosts via SSH to install its Java process

## Pipelines

* Let you run job config from a **Jenkinsfile**
* Let's you build **Stages** (i.e. sub-jobs) and then run them in parallel
  * One Stage can stash some files to later unstash in other stages
  * Stages can be run across many workers for parallelism

## Labels

* Workers can have labels to denote what kinds of things they can run
  * e.g. Different OSes, different programs installed, etc.

## Modules

* Extension plugins for Jenkins
* Register callbacks into the Jenkins process
* e.g. For git, Slack, etc.
