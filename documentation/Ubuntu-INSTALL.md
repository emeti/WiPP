# WiPP - Installation on Ubuntu

## Introduction

This install guide is available at [github.com/emeti/WiPP/Ubuntu-INSTALL.md](https://github.com/emeti/WiPP/Ubuntu-INSTALL.md)

The reason for the existence of the instructions below is that we had encountered several [issues](https://github.com/bihealth/WiPP/issues)  trying to install the original [WiPP workflow](https://github.com/bihealth/WiPP) and had to document proper environment to make it work.

Prerequisites below have been tested with Ubuntu 64-bit 16.04.6 in a VM.

There is also a CentOS based docker image and docker container which captures a working and minimum necessary environment for the  TR and PP parts of the flow. As currently defined the AN step requires a GUI and a PDF reader.

## Setup or get access to an Ubuntu compute instance

Following advice from [README](./REAMDE.md) only Ubuntu version 16 has been attempted. 

I do not see a real reason that the builds of R libraries cannot be done with newer glibc versions and run in Ubuntu 18, but have not attempted them as that certainly is more work.


### Hardware requirements

CPUs (cores) - 4
Memory - 8 GB
Disk size - 50 GB 

There were some problems with using the default VMware Fusion VM settings for Ubuntu of 20GB disk / 1 CPU / small Mem . 
The problems were with conda environments environments (running out of disk space warnings) and with workflow runs (snakemake errors) - disappeared with resources increase outline above.

## Install conda

At the time of writing  there are [issues with snakemake and conda]() - there are some hacks to make new conda work with curent snakemake but they are all dirty hacks and would lead to further problems).

This problem has been address by using an older version conda, the one which still is using the old (and non-portable/conflicing ) ``source activate`` instead of ``conda activate`` command for switching environments.

Here is conda install 

```
curl -sSL https://repo.anaconda.com/miniconda/Miniconda3-4.3.31-Linux-x86_64.sh -o miniconda3.sh
sh miniconda3.sh
```

## Install libnetcdf

I think this part is not necessary, unless separate software apart from the WiPP workflow needs this. 
It is an incorrect statement in the original WiPP instructions.
All required libraries are pulled by conda using a proper (frozen) list of dependencies from a new R\_env.yaml file.

## Install git and curl

```
sudo apt-get update
sudo apt install git
sudo apt install curl
```

The versions used further
```
$ git --version
git version 2.7.4
$ curl --version
curl 7.47.0 (x86_64-pc-linux-gnu)
...
``` 

curl dependency used to obtain conda below is optional, but very useful

## Clone WiPP repository

```
wget -O WiPP-master.zip https://github.com/emeti/WiPP/archive/master/zip
unzip WiPP-master.zip
rm WiPP-master.zip
cd WiPP
```

The natural ``git clone https://[WiPP repo]`` (i.e. not an ssh version) sometimes does not work (see stackexchanges for various reasons) hence we use wget to avoid problems.

## R\_env.yaml

Make sure you use a modified R libraries dependencies. If WiPP is cloned from an EMETI fork of WiPP you will use the recently  tested R lib environment settings.

## Run WiPP
Follow the instructions from a [README](../README.md) 


