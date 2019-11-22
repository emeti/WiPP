# WiPP install on CentOS

Download CentOS 7 installation image and install CentOS 7 on a VM.
Or obtain access to a VM ready with CentOS 7.


## Intstall required tools

Open terminal with shell. Your account needs to be in sudo-ers group (admin priviledges) to install some prerequisites (git).

### Install git

```
sudo yum install git
```
Yum is the default package manger in CentOS. The first command refreshes its pacakge index. 

Why git ? Apparently git is necessary for the workflow.

What version of git has been installed?
```
$ git --version
git version 1.8.3.1
```

Normally curl is installed in CentOS. If not, you might need to ``yum install`` it.

The rest of the steps is the same as in Ubuntu installation 

See the [Ubuntu-INSTALL instructions](./Ubuntu-INSTALL.md)


