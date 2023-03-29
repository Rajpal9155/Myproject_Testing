![AMDh_mb_8bit](media/image1.png){width="2.175in"
height="0.8333333333333334in"}

  -----------------------------------------------------------------------
  AMD ROCm™ Getting Started Guide for Linux

  -----------------------------------------------------------------------

  ---------- ------------- ------- ----- ----------- ----- ------ ---- ---- ---- ---- ----- ----------
             Publication           1.0   Revision:         0330                             
             \#                                                                             

             Issue Date:   March                                                            
                           2022                                                             

                                                                                            
  ---------- ------------- ------- ----- ----------- ----- ------ ---- ---- ---- ---- ----- ----------

© 2022 Advanced Micro Devices, Inc. All Rights Reserved.

[]{#_Toc503941611 .anchor}Table of Contents

[Table of Contents [2](#_Toc503941611)](#_Toc503941611)

[Chapter 1 Overview of ROCm Installation
[3](#overview-of-rocm-installation)](#overview-of-rocm-installation)

[About This Document [3](#about-this-document)](#about-this-document)

[System Requirements [3](#system-requirements)](#system-requirements)

[Prerequisites [4](#prerequisites)](#prerequisites)

[Chapter 2 How to Install ROCm
[6](#how-to-install-rocm)](#how-to-install-rocm)

[2.1 AMDGPU and ROCm Stack Repositories Base URLs
[6](#amdgpu-and-rocm-stack-repositories-base-urls)](#amdgpu-and-rocm-stack-repositories-base-urls)

[2.1.1 Ubuntu 18.04 / 20.04
[6](#ubuntu-18.04-20.04)](#ubuntu-18.04-20.04)

[2.1.2 CentOS/RHEL 7.9 [6](#centosrhel-7.9)](#centosrhel-7.9)

[2.1.3 CentOS /RHEL 8.4 [6](#centos-rhel-8.4)](#centos-rhel-8.4)

[2.1.4 RHEL 8.5 [7](#rhel-8.5)](#rhel-8.5)

[2.1.5 SLES 15 Service Pack 3
[7](#sles-15-service-pack-3)](#sles-15-service-pack-3)

[2.2 Install ROCm [7](#install-rocm)](#install-rocm)

[2.2.1 ROCm Installation on Ubuntu
[7](#rocm-installation-on-ubuntu)](#rocm-installation-on-ubuntu)

[2.2.2 ROCm Installation on RHEL/CentOS
[9](#rocm-installation-on-rhelcentos)](#rocm-installation-on-rhelcentos)

[2.2.3 ROCm Installation on SLES/OpenSUSE
[11](#rocm-installation-on-slesopensuse)](#rocm-installation-on-slesopensuse)

[2.2.4 Verification Process
[13](#verification-process)](#verification-process)

[Chapter 3 Post Install Actions
[14](#post-install-actions)](#post-install-actions)

[Chapter 4 ROCm Stack Uninstallation
[15](#rocm-stack-uninstallation)](#rocm-stack-uninstallation)

# Overview of ROCm Installation

## About This Document {#about-this-document .unnumbered}

This document is intended for advance users and provides fresh and clean
installation / uninstallation instructions on various flavors of Linux
distributions.

For more details on ROCm upgrade, refer to the ROCm Installation Guide
at:

<https://docs.amd.com>

**This document also refers to Radeon™ Software for Linux® as AMDGPU
stack, including the kernel-mode driver *amdgpu-dkms*.**

The guide provides the instructions for the following:

-   Kernel-mode Driver Installation

```{=html}
<!-- -->
```
-   ROCm single and multi-version installation

-   ROCm single and multi-version uninstallation

-   Kernel-mode Driver Uninstallation

## System Requirements {#system-requirements .unnumbered}

***Linux Distribution Requirement***

  -----------------------------------------------------------------------
  OS-Version (64-bit)                    Kernel Versions
  -------------------------------------- --------------------------------
  CentOS 8.4                             4.18.0-305.25.1.el8_4.x86_64

  CentOS 7.9                             3.10.0-1160  

  RHEL 8.5                               4.18.0-348.2.1.el8_5.x86_64

  RHEL 7.9                               3.10.0-1160.el7.x86_64

  SLES 15 SP3                            5.3.18-59.19-default

  Ubuntu 20.04.4                         5.13.0-35-generic

  Ubuntu 18.04.5 \[5.4 HWE kernel\]      5.4.0-91-generic
  -----------------------------------------------------------------------

***GPU Requirement***

  ------------------------------------------------------------------------
  Classification             GPU Name                 Product Id
  -------------------------- ------------------------ --------------------
  GFX9 GPUs                  AMD Radeon Instinct™     Vega 20
                             MI50                     

                             AMD Radeon Instinct™     
                             MI60                     

                             AMD Radeon™ Pro VII      

  RDNA GPUs                  AMD Radeon™ Pro W6800    Navi 21 GL-XL

                             AMD Radeon™ Pro V620     Navi 21 GL-XE

  CDNA GPUs                  AMD Instinct™ MI100      Arcturus

                             AMD Instinct™ MI200      Aldebaran
  ------------------------------------------------------------------------

## Prerequisites {#prerequisites .unnumbered}

**#Verify System requirements are met**

#Verify if linux distribution is supported

\$ uname -m && cat /etc/\*release

#Verify Kernel Version is supported

\$ uname -srmv

#Verify if ROCm System has ROCm supported GPU

lspci \| grep -i display

#Enable Additional Repositories

#For CentOS 7&8: 

sudo yum install -y epel-release

#RHEL 7.x:

wget
https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
&& sudo rpm -ivh epel-release-latest-7.noarch.rpm

#For RHEL 8.x:

wget
https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
&& sudo rpm -ivh epel-release-latest-8.noarch.rpm

#For CentOS 8.3 Only:

\$ sudo yum config-manager \--set-enabled powertools

#For RHEL 8.3/8.4/8.5:

\$ sudo subscription-manager repos
\--enable codeready-builder-for-rhel-8-x86_64-rpms

#Verify System has required tools and packages installed

#Ubuntu/Debian

\$ sudo apt list \--installed \| grep \'wget\\\|gnupg2\'

#RHEL/CentOS

\$ sudo yum list installed \| grep wget

#SLES

\$ sudo zypper search \--installed-only \| grep wget

#Install required tools and packages if installation does not exist

#Ubuntu/Debian

\$ sudo apt-get update

\$ sudo apt-get install wget gnupg2

#RHEL/CentOS

\$ sudo yum clean all

\$ sudo yum install wget

#SLES

\$ zypper install wget

#Setting Permissions for Groups

#Check the existing groups on your system

\$ groups

#Add yourself to video group to access GPU resources

\$ sudo usermod -a -G video \$LOGNAME

#To add future users to the video and render groups

\$ echo \'ADD_EXTRA_GROUPS=1\' \| sudo tee -a /etc/adduser.conf

\$ echo \'EXTRA_GROUPS=video\' \| sudo tee -a /etc/adduser.conf

\$ echo \'EXTRA_GROUPS=render\' \| sudo tee -a /etc/adduser.conf

# How to Install ROCm

## AMDGPU and ROCm Stack Repositories Base URLs

### Ubuntu 18.04 / 20.04

#### Repositories with Latest Packages

amdgpu base url= <https://repo.radeon.com/amdgpu/latest/ubuntu>

rocm base url= <https://repo.radeon.com/rocm/apt/debian/>

#### Repositories for Specific Releases

*amdgpu base url=* <https://repo.radeon.com/amdgpu/22.10/ubuntu>

*rocm base url=* <https://repo.radeon.com/rocm/apt/5.1>

### CentOS/RHEL 7.9

#### Repositories with Latest Packages

amdgpu base url=
<https://repo.radeon.com/amdgpu/latest/rhel/7.9/main/x86_64>

rocm base url= <https://repo.radeon.com/rocm/yum/rpm>

#### Repositories for Specific Releases

*amdgpu base url=*
<https://repo.radeon.com/amdgpu/22.10/rhel/7.9/main/x86_64>

*rocm base url=* [https://repo.radeon.com/rocm/yum/5.1/main
/](https://repo.radeon.com/rocm/yum/5.1/main%20/)

### CentOS /RHEL 8.4

#### Repositories with Latest Packages

amdgpu base url=
<https://repo.radeon.com/amdgpu/latest/rhel/8.4/main/x86_64>

rocm base url= <https://repo.radeon.com/rocm/centos8/rpm>

#### Repositories for Specific Releases

amdgpu base url=
<https://repo.radeon.com/amdgpu/22.10/rhel/8.4/main/x86_64>

rocm base url= <https://repo.radeon.com/rocm/centos8/5.1/main/>

### RHEL 8.5

#### Repositories with Latest Packages

*amdgpu base url=*
<https://repo.radeon.com/amdgpu/latest/rhel/8.5/main/x86_64>

*rocm base url=* <https://repo.radeon.com/rocm/centos8/rpm>

#### Repositories for Specific Releases

amdgpu base url=
<https://repo.radeon.com/amdgpu/22.10/rhel/8.5/main/x86_64>

rocm base url= <https://repo.radeon.com/rocm/centos8/5.1/main/>

### SLES 15 Service Pack 3

#### Repositories with Latest Packages

amdgpu base url=
<https://repo.radeon.com/amdgpu/latest/sle/15/main/x86_64>

rocm base url= <https://repo.radeon.com/rocm/zyp/zypper/>

#### Repositories for Specific Releases

amdgpu base url=
<https://repo.radeon.com/amdgpu/22.10/sle/15/main/x86_64>

rocm base url= <https://repo.radeon.com/rocm/zyp/5.1/main/>

## Install ROCm

**NOTE:** The ROCm installation instructions in this section use AMDGPU
and ROCm repositrory base URLs pointing to Repositories for Specific
Release. However, users can also use base URLs pointing to Repositories
with Latest Packages given in the [AMDGPU and ROCm Stack Repositories
Base URLs](#amdgpu-and-rocm-stack-repositories-base-urls) section.

### ROCm Installation on Ubuntu

**#Check if Kernel Headers and development packages installed on
system**

*\$ sudo dpkg -l \| grep linux-headers*

\$ sudo dpkg -l \| grep linux-modules-extra

**#If installation does not exist, Install Kernel headers & Development
Packages**

\$ sudo apt install linux-headers-\`uname -r\`
linux-modules-extra-\`uname -r\`

**#Install ROCm**

*#Add gpg key and AMDGPU Repository*

\$ wget -q -O - https://repo.radeon.com/rocm/rocm.gpg.key \| sudo
apt-key add -

**NOTE:** The **gpg** key may change. Ensure it is updated when
installing a new release. If the key signature verification fails while
updating, re-add the key from the ROCm *apt* repository as mentioned
above. The current *rocm.gpg.key* is not available in a standard key
ring distribution.

73f5d8100de6048aa38a8b84cd9a87f05177d208 rocm.gpg.key

**Ubuntu 18.04**

\$ echo \'deb \[arch=amd64\]
<https://repo.radeon.com/amdgpu/22.10/ubuntu> bionic main\' \| sudo tee
/etc/apt/sources.list.d/amdgpu.list

**Ubuntu 20.04**

\$ echo \'deb \[arch=amd64\]
<https://repo.radeon.com/amdgpu/22.10/ubuntu> focal main\' \| sudo tee
/etc/apt/sources.list.d/amdgpu.list

#Update the package list

\$ sudo apt-get update

\# Install the Kernel Mode Driver and Reboot System

\$ sudo apt install amdgpu-dkms

\$ sudo reboot

\# *Add ROCm Repository*

\$ echo \'deb \[arch=amd64\] <https://repo.radeon.com/rocm/apt/5.1>
ubuntu main\' \| sudo tee /etc/apt/sources.list.d/rocm.list

\$ sudo apt-get update

**#Install Single Version ROCm Meta-packages**

\$ sudo apt install \<package-name\>

**#Install Multi-version ROCm Meta-packages**

#Add desired ROCm repositories

*\$* *echo* *\'deb \[arch=amd64\]*
<https://repo.radeon.com/rocm/apt/5.0> *ubuntu main\'* \| *sudo tee
/etc/apt/sources.list.d/rocm.list*

*\$* *echo* *\'deb \[arch=amd64\]*
<https://repo.radeon.com/rocm/apt/5.1> *ubuntu main\'* \| *sudo tee
/etc/apt/sources.list.d/rocm.list*

*\$ sudo apt update*

#Install Multi-version meta-packages

*\$ sudo apt install \<package-name with 5.0 version\> \<package-name
with 5.1 version\>*

*\
*

### ROCm Installation on RHEL/CentOS 

**#Check if Kernel Headers and development packages installed on
system**

\$ sudo yum list installed \| grep linux-headers

\$ sudo yum list installed \| grep linux-modules-extra

**#If installation does not exist, Install Kernel headers & Development
Packages**

\$ sudo yum install kernel-headers-\`uname -r\` kernel-devel-\`uname
-r\`

#Preparing RHEL 7.9 for Installation

#Enable repositories

\$ sudo subscription-manager repos \--enable rhel-server-rhscl-7-rpms

\$ sudo subscription-manager repos \--enable rhel-7-server-optional-rpms

\$ sudo subscription-manager repos \--enable rhel-7-server-extras-rpms

\$ sudo subscription-manager repos \--enable=rhel-7-server-devtools-rpms

#Preparing CentOS for Installation

#For CentOS 7&8: 

\$ sudo yum install epel-release

#Only for CentOS 7.9

\$ sudo yum install -y centos-release-scl

#Installing Devtoolset-7 for RHEL 7.9 /CentOS 7.9

\$ sudo yum install devtoolset-7

\$ source scl_source enable devtoolset-7

**#Install ROCm**

#Add the AMDGPU Stack Repository- Create a /etc/yum.repos.d/amdgpu.repo
file with the following content:

\[amdgpu\]

name=amdgpu

baseurl=<https://repo.radeon.com/amdgpu/22.10/rhel/8.5/main/x86_64>

enabled=1

gpgcheck=1

gpgkey=https://repo.radeon.com/rocm/rocm.gpg.key

**Note:** The gpg key may change; ensure it is updated when installing a
new release. If the key signature verification fails while updating,
re-add the key from the ROCm to the yum repository as mentioned above.
The current rocm.gpg.key is not available in a standard key ring
distribution but has the following sha1sum hash:

73f5d8100de6048aa38a8b84cd9a87f05177d208 rocm.gpg.key

\#*C*lean the cached files from enabled repositories

\$ sudo yum clean all

\# Install the Kernel Mode Driver and Reboot System

\$ sudo yum install amdgpu-dkms

\$ sudo reboot

#Add the ROCm Stack Repository- Create a /etc/yum.repos.d/rocm.repo file
with the following content:

\[rocm\]

name=rocm

baseurl=<https://repo.radeon.com/rocm/centos8/5.1/main/>

enabled=1

gpgcheck=1

gpgkey=https://repo.radeon.com/rocm/rocm.gpg.key

\#*C*lean the cached files from enabled repositories

\$ sudo yum clean all

**#Install Single Version ROCm Meta-packages**

\$ sudo yum install \<package-name\>

**#Install Multi-version ROCm Meta-packages**

\# Add desired ROCm repositories- Create a /etc/yum.repos.d/rocm.repo
file with the following content:

\[ROCm-1\]

Name=ROCm5.0

baseurl=<https://repo.radeon.com/rocm/centos8/5.0/>

enabled=1

gpgcheck=1

gpgkey=https://repo.radeon.com/rocm/rocm.gpg.key

\[ROCm-2\]

Name=ROCm5.1

baseurl=<https://repo.radeon.com/rocm/centos8/5.1/main/>

enabled=1

gpgcheck=1

gpgkey=https://repo.radeon.com/rocm/rocm.gpg.key

\#*C*lean the cached files from enabled repositories

\$ sudo yum clean all

#Install multiversion meta-packages

*\$ sudo yum install \<package-name5.0\> \<package-name5.1\>*

### ROCm Installation on SLES/OpenSUSE 

**#Check if Kernel Headers and development packages installed on
system**

*\$ sudo zypper info kernel-default-devel or kernel-default*

**#If installation does not exist, Install Kernel headers & Development
Packages**

*\$ sudo zypper install kernel-default-devel or kernel-default*

**#ROCm Installation**

#Add the AMDGPU Stack Repository- Create a /etc/zypp/repos.d/amdgpu.repo
file with the following content:

\[amdgpu\]

name=amdgpu

baseurl=<https://repo.radeon.com/amdgpu/22.10/sle/15/main/x86_64>

enabled=1

gpgcheck=1

gpgkey=https://repo.radeon.com/rocm/rocm.gpg.key

**NOTE*:*** The gpg key may change; ensure it is updated when installing
a new release. If the key signature verification fails while updating,
re-add the key from the ROCm zypp repository as mentioned above. The
current rocm.gpg.key is not available in a standard key ring
distribution but has the following sha1sum hash:

73f5d8100de6048aa38a8b84cd9a87f05177d208 rocm.gpg.key

#Update the added repository and add perl repository:

\$ sudo zypper ref

\$ sudo zypper clean \--all

\$ sudo zypper addrepo
<https://download.opensuse.org/repositories/devel:languages:perl/SLE_15/devel:languages:perl.repo>

\$ sudo SUSEConnect -p sle-module-desktop-applications/15.3/x86_64

\$ sudo SUSEConnect \--product sle-module-development-tools/15.3/x86_64

\$ sudo SUSEConnect \--product PackageHub/15.3/x86_64

\$ sudo zypper ref

#Install the Kernel Mode Driver and Reboot System

\$ sudo zypper \--gpg-auto-import-keys install amdgpu-dkms

\$ sudo reboot

#Add the ROCm Stack Repository

\[*rocm\]*

*name=rocm*

*baseurl=*<https://repo.radeon.com/rocm/zyp/5.1/main/>

*enabled=1*

*gpgcheck=1*

*gpgkey=*https://repo.radeon.com/rocm/rocm.gpg.key

\$ sudo zypper ref

**#Install Single Version ROCm Meta-packages**

\$ sudo zypper \--gpg-auto-import-keys install \<package-name\>

**#Install Multi-Version ROCm Meta-packages**

#Add desired ROCm repositories

\[*rocm-1\]*

*name=Rocm5.0*

*baseurl=*<https://repo.radeon.com/rocm/zyp/5.0/>  

*enabled=1*

*gpgcheck=1*

*gpgkey=*https://repo.radeon.com/rocm/rocm.gpg.key

\[*rocm-2\]*

*name=Rocm5.1*

*baseurl=*<https://repo.radeon.com/rocm/zyp/5.1/main/>

*enabled=1*

*gpgcheck=1*

*gpgkey=*https://repo.radeon.com/rocm/rocm.gpg.key

\$ sudo zypper ref

\$ sudo zypper \--gpg-auto-import-keys install
\<package-name5.0\>\<package-name5.1\>

### Verification Process

#Verify if Kernel mode driver installed successfully

\$ sudo dkms status

#Verifying ROCm Installation -Check if GPUs are listed

/opt/rocm-\<version\>/bin/rocminfo

OR

/opt/rocm-\<version\>/opencl/bin/clinfo

#### Verify Installed Packages

  -----------------------------------------------------------------------
  Linux Distro           Command
  ---------------------- ------------------------------------------------
  Ubuntu/Debian          \$ sudo apt list \--installed

  RHEL/CentOS            \$ sudo yum list installed

  OpenSUSE / SLES        \$ sudo zypper search \--installed-only
  -----------------------------------------------------------------------

# Post Install Actions

#Users can set LD_LIBRARY_PATH to load the ROCm library version of
choice.

\$ export
LD_LIBRARY_PATH=/opt/rocm-\<version\>/lib;/opt/rocm-\<version\>/lib64

**Note:** For convenience, users may add the ROCm binaries in your PATH,
as shown in the example below.

\$ echo 'export
PATH=\$PATH:/opt/rocm-\<version\>/bin:/opt/rocm-\<version\>/opencl/bin'

# ROCm Stack Uninstallation

***#Uninstalling Specific Meta-packages***

#UBUNTU/DEBIAN

#Uninstall single version ROCm packages

\$ sudo apt autoremove \<package-name\>

#Uninstall multi-version ROCm packages

\$ sudo apt autoremove \<package-name with release version\>

#RHEL/CentOS

#Uninstall single version ROCm packages

\$ sudo yum remove \<package-name\>

#Uninstall multi-version ROCm packages

\$ sudo yum remove \<package-name with release version\>

#SLES/OPENSUSE

#Uninstall single version ROCm packages

\$ sudo zypper remove \<package-name\>

#Uninstall multi-version ROCm packages

\$ sudo zypper remove \<package-name with release version\>

***#Complete Uninstallation of ROCm Packages***

**\#**UBUNTU/DEBIAN

#Uninstall all single version ROCm packages

\$ sudo apt autoremove rocm-core

#Uninstall all multi-version ROCm packages

\$ sudo apt autoremove rocm-core\<release version\>

RHEL/CentOS

#Uninstall all single version ROCm packages

\$ sudo yum remove rocm-core

#Uninstall all multi-version ROCm packages

\$ sudo yum remove rocm-core\<release version\>

SLES/OPENSUSE

#Uninstall all single version ROCm packages

\$ sudo zypper remove rocm-core

#Uninstall all multi-version ROCm packages

\$ sudo zypper remove rocm-core\<release version\>

***#Uninstall Kernel Mode Driver***

UBUNTU/DEBIAN

\$ sudo apt autoremove amdgpu-dkms

RHEL/CentOS

\$ sudo yum remove amdgpu-dkms

SLES/OPENSUSE

\$ sudo zypper remove amdgpu-dkms

***#Remove ROCm and AMDGPU Repositories***

UBUNTU/DEBIAN

\$ sudo rm /etc/apt/sources.list.d/\<rocm_repository-name\>.list

\$ sudo rm /etc/apt/sources.list.d/\<amdgpu_repository-name\>.list

#Clear cache and clean the system.

\$ sudo rm -rf /var/cache/apt/\*

\$ sudo apt-get clean all

#Reboot the system

\$ sudo reboot

RHEL/CentOS

\$ sudo rm -rf /etc/yum.repos.d/\<rocm_repository-name\> # Remove only
rocm repo

\$ sudo rm -rf /etc/yum.repos.d/\<amdgpu_repository-name\> # Remove only
amdgpu repo

#Clear cache and clean the system.

\$ sudo rm -rf /var/cache/yum   

\$ sudo yum clean all

#Restart the system.

\$ sudo reboot

SLES/OPENSUSE

\$ sudo zypper removerepo \<rocm_repository-name\>

\$ sudo zypper removerepo \<amdgpu_repository-name\>

**#Clear cache and clean the system**

\$ sudo zypper clean --all

#Restart the system.

\$ sudo reboot
