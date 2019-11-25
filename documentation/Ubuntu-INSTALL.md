# WiPP - Installation on Ubuntu

## Introduction

This install guide is available at [github.com/emeti/WiPP/Ubuntu-INSTALL.md](https://github.com/emeti/WiPP/Ubuntu-INSTALL.md)

The reason for the existence of the instructions below is that we had encountered several [issues](https://github.com/bihealth/WiPP/issues)  trying to install the original [WiPP workflow](https://github.com/bihealth/WiPP) and had to document proper environment to make it work.

Prerequisites below have been tested with Ubuntu 64-bit 16.04.6 in a VM.

There is also a CentOS based docker image and docker container which captures a working and minimum necessary environment for the  TR and PP parts of the flow. As currently defined the AN step requires a GUI and a PDF reader.

## Setup or get access to an Ubuntu compute instance

Following advice from [README](./REAMDE.md) only Ubuntu version 16 has been attempted. 

I do not see a real reason that the builds of R libraries cannot be done with newer glibc versions and run in Ubuntu 18, but have not attempted them as that certainly is more work.

### Create a VM with Ubuntu OS

Make sure to use version 16 of Ubuntu

Local installation in VMWare or Virtual Box:

1. Goto [Ubuntu alternative downloads](https://ubuntu.com/download/alternative-downloads) and download the operating system installer ISO image
2. Create a new virtual machine , start the installer, create a user, select packages

Terminal with shell will be the main execution interface.

### Hardware requirements

CPUs (cores) - 4

Memory - 8 GB

Disk size - 50 GB 

There were some problems with using the default VMware Fusion VM settings for Ubuntu of 20GB disk / 1 CPU / small Mem . 
The problems were with conda environments (running out of disk space warnings) and with workflow runs (snakemake errors) - disappeared with resources increase outline above.

## Install git

```
sudo apt-get update
sudo apt install git
```
Why git ? Apparently git is required by the snakemake part of the workflow. Question: Why? Answer: Ask the authors.

## Install curl (optional)

```
sudo apt install curl
```

If you want to skip curl install to keep the build minimal, then you would need to modify the command downloading conda used in the subsequent step and use wget instead of curl. I like to have curl handy as a very powerfull tool though.

The versions used further
```
$ git --version
git version 2.7.4
$ curl --version
curl 7.47.0 (x86_64-pc-linux-gnu)
...
``` 

The curl dependency used to obtain conda below is optional.


## Install conda

At the time of writing  there are [still unresolved issues with snakemake](https://bitbucket.org/snakemake/snakemake/issues/1115/cannot-activate-conda-enironment-using ) a result of the way snakemake uses conda - there are some hacks to make new snakemake work with latest conda version but they are all dirty hacks and would inevitably lead to further problems.

In a dedicated VM or a container we can address this issue by using an older version conda - the one which still is using the old (and non-portable/conflicing ) ``source activate`` instead of ``conda activate`` command for switching environments.

Here is what to do to install an older (mini)conda version 

```
curl -sSL https://repo.anaconda.com/miniconda/Miniconda3-4.3.31-Linux-x86_64.sh -o miniconda3.sh
sh miniconda3.sh
```

If you need to use a newer version of conda beware of the problems.

## Install libnetcdf

There is no need to install [netcdf library](https://www.unidata.ucar.edu/software/netcdf/) via apt-get (Ubuntu package manager), unless you really need a system-wide install or have other software using the same library.

For a user level install in separate vm or a docker contaienr all necessary libraries, including netcdf, are pulled by conda using a frozen list of dependencies from a correct R\_env.yaml file (or possibly WiPP.yml).


## Clone or copy WiPP repository

```
wget -O WiPP-master.zip https://github.com/emeti/WiPP/archive/master.zip
unzip WiPP-master.zip
rm WiPP-master.zip
cd WiPP-master
```

It is also possible to clone the repo using git - see [README](../README.md)

## Note about R\_env.yaml

This R\_env.yaml file is used by the WiPP scripts / snakemake / conda. It contains critical environment dependencies defintion and correctness of it is necessary for the workflow to work. Make sure you use a modified R libraries dependencies. There is a way to test it and modify it separately (e.g. to prepare for upgrades) using conda withoug running the whole workflow (time consuming). Please contact the author for hints if you need to do that. 

If your local  WiPP (or WiPP-master) directory contains code cloned from an EMETI fork of WiPP you will be using the recently tested R lib environment settings.

## Run WiPP
Follow the instructions from a [README](../README.md) starting from the Run point.

