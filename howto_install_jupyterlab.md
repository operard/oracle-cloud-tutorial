# How to install Jupyterlab in Oracle Cloud (OCI) ?

The following walk-through guides you through the steps needed to set up your environment to run Jupyterlab in Oracle Cloud Infrastructure.

## Prerequisites

You have deployed a VM 2.1 with Oracle Linux 7.9 (OEL7) in Oracle Cloud Infrastructure (OCI).

- The installation of Oracle Linux 7.9 is using pip3.6 by default. 
- Python 3.6 or higher is installed
- You have access to root either directly or via sudo. By default in OCI, you are connected like "opc" user with sudo privilege.

## Jupyterlab Installation

The install is pretty simple. It consists of setting up python, installing python components and libraries. 
Lets start with setting up the Python Environment

# Install virtualenv
sudo pip3.6 install virtualenv
# Create an environment for Redbull HOL
virtualenv -p /usr/bin/python3 redbullenv
# Activate the env
source redbullenv/bin/activate

# check list of Python Libraries in your environment
(redbullenv) [opc@redbull-lab1 ~]$ pip3 list
Package    Version
---------- -------
pip        21.1.3
setuptools 57.1.0
wheel      0.36.2
WARNING: You are using pip version 21.1.3; however, version 21.2.1 is available.
You should consider upgrading via the '/home/opc/redbullenv/bin/python -m pip install --upgrade pip' command.


# Install git to connect to github
sudo yum install git

# install jupyterlab
pip3 install jupyterlab

# Upgrade Environment
/home/opc/redbullenv/bin/python -m pip install --upgrade pip


# Libraries to install

pip install pandas
pip install pandarallel
pip install dask
pip install seaborn
pip install matplotlib
pip install plotly

pip install kafka-python (v2.0.0)

pip install -lxml==4.6.3
pip install selenium
pip install beautifulsoup4

pip install scikit-learn

pip install Flask
pip install gunicorn

# Install extensiones for jupyterlab
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
jupyter nbextension enable execute_time/ExecuteTime


# Create script to instantiate automatically  reboot jupyterlab with opc user "launchjupyterlab.sh"
  4 -rwxrwxrwx.  1 opc  opc     284 Jul 28 20:21 launchjupyterlab.sh

# Script

#!/bin/bash

#source /home/oracle/.bashrc
# Activate Redbull Environment
source redbullenv/bin/activate

cd /home/opc

if [ "$1" = "start" ]
then
nohup jupyter-lab --ip=0.0.0.0 --port=8001 > ./nohup.log 2>&1 &
echo $! > /home/opc/jupyter.pid
else
kill $(cat /home/opc/jupyter.pid)
fi

# connect to root user
sudo -i

# create script to start, stop service "jupyterlab"

 vi /etc/systemd/system/jupyterlab.service

# Add next lines to launch like "opc" user the script "launchjupyterlab.sh"

[Unit]
Description=Service to start jupyterlab for opc
Documentation=
[Service]
User=opc
Group=opc
Type=forking
WorkingDirectory=/home/opc
ExecStart=/home/opc/launchjupyterlab.sh start
ExecStop=/home/opc/launchjupyterlab.sh stop
[Install]
WantedBy=multi-user.target

# Test Service

systemctl start jupyterlab
systemctl status jupyterlab
systemctl enable jupyterlab

# reboot machine to check that jupyterlab is enabled by default on port 8001

# Open ports to the machine VM2.1
firewall-cmd  --permanent --zone=public --list-ports
firewall-cmd --get-active-zones
firewall-cmd --permanent --zone=public --add-port=8001/tcp
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --reload


