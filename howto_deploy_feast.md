# How to deploy the open-source Feature Store Feast on Oracle Cloud (OCI)? TODO

The following walk-through guides you through the steps needed to set up your FEAST environment to run FEAST in Oracle Cloud Infrastructure.

## Prerequisites

You have deployed a VM 2.1 with Oracle Linux 7.9 (OEL7) in Oracle Cloud Infrastructure (OCI).

- The installation of Oracle Linux 7.9 is using pip3.6 by default. 
- Python 3.6 or higher is installed
- You have access to root either directly or via sudo. By default in OCI, you are connected like "opc" user with sudo privilege.

## FEAST Installation

The install is pretty simple. It consists of setting up python, installing python components and libraries. 
Lets start with setting up the Python Environment



### Connect to "root" user

```console
sudo -i
```

### Create a script to start, stop service "FEAST"

```console
vi /etc/systemd/system/feast.service
```


