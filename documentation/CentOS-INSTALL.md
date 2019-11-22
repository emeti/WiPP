# WiPP install on CentOS

Download CentOS 7 installation image and install CentOS 7 on a VM.
Or obtain access to a VM ready with CentOS 7.


## Intstall required tools

Open terminal with shell. This is your main interface for installing required tools as well as starting and interacting with the workflow.

Your account needs to be in sudo-ers group (admin priviledges) to install some prerequisites (git).

### Install git

```
sudo yum install git
```
Yum is the default package manger in CentOS.

Git is required by the workflow script and/or snakemake it is using.

What version of git has been installed?
```
$ git --version
git version 1.8.3.1
```

Normally curl is installed in CentOS. If not, you might need to ``yum install`` it as well.

The rest of the steps is the same as outlined in the Ubuntu installation 

See the [Ubuntu-INSTALL instructions](./Ubuntu-INSTALL.md)

Basically - one needs to install an appropriate version of conda, get the WiPP repo copy and follow the steps from the main 
[README](../README.md)
