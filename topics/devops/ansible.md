# Ansible Notes

* Library to SSH onto machines and run commands

## Playbooks

* Definition of what Ansible should do
* Composed of *Plays*

## Plays

* Has name, runs on a set of hosts (one or more)
* Defines *Roles*

## Roles

* They're like functions that contain the *Tasks* you want to do
* Can take arguments to define variable within a task
* Starts in `main.yml` to define task sequence

## Tasks

* Each task defines one or more *Modules* to run
* Are idempotent

## Modules

* What runs the system commands
  * e.g. `apt` module for `apt-get`
  * e.g. `lineinfile` file to modify files
* Result of a module can be "Okay" (nothing changed), "Changed" (had to do something), or "Failed"

## Inventories

* Define what hosts are available

## Templates

* Used for creating files to copy to a host
  * e.g. To create `upstart` scripts
