---
title: Using Ansible and DSC with Windows
description: Using Ansible and DSC in a Windows environment
categories:
  - ansible
  - dsc
  - windows
  - centos
toc: false
comments: true
---

## Scenario

For the past year or so I've been teaching my friend [Steve](https://github.com/steevaavoo) about the many tools
and techniques I've been using at
work, including:

- Git
- Vagrant
- Packer
- PowerShell (various topics like Best Practices, Module Design, Build Automation)
- Desired State Configuration (DSC)

We gradually built upon each topic until we had a working build for Exchange:
[https://github.com/steevaavoo/ExchangeLab](https://github.com/steevaavoo/ExchangeLab)

This all worked great as an example, but along the way we stumbled across several frustrations with DSC:

- The tooling isn't as complete as other more mature Configuration Management solutions.
- Using DSC with the Local Configuration Manager (LCM) is not as flexible as other solutions.
- Getting runtime data is not easy. Eg.
  [Dynamically getting a certificate thumprint after creating it](https://github.com/steevaavoo/ExchangeLab/issues/3)
  
We needed a solution that would overcome the above shortcomings of our current method using standard DSC / LCM.

## Solution

[Ansible](https://www.ansible.com/) is a simple, agent-less configuration management tool, with excellent
[documentation](https://docs.ansible.com/) and [community support](https://www.ansible.com/community).

There are many more features available with Ansible, making it more flexible when building Playbooks (configurations).

I've created a repo for testing Ansible and DSC on Windows, with a view to port the current Exchange configuration over and add the missing functionality to the build.

### Ansible Control VM

Installing Ansible was as simple as adding these lines to the [Vagrantfile configuration](https://github.com/adamrushuk/Ansible-Windows/blob/master/Vagrantfile#L53-L62):

```bash
yum -y install epel-release
yum -y install ansible
yum -y install python-pip
yum -y install tree
pip install --upgrade pip
pip install pywinrm
```

### Windows VMs

Building the Windows VMs also only required a few provisioning steps in their [Vagrantfile configuration](https://github.com/adamrushuk/Ansible-Windows/blob/master/Vagrantfile#L88-L94):

```bash
    # Provisioning
    # Reset Windows license
    subconfig.vm.provision 'shell', inline: 'cscript slmgr.vbs /rearm //B //NOLOGO'
    # Configure remoting for Ansible
    subconfig.vm.provision 'shell', path: 'Vagrant/Scripts/ConfigureRemotingForAnsible.ps1'
    # Reboot VM
    subconfig.vm.provision :reload
```

**NOTE:** There were issues with the latest versions of VirtualBox (5.2.22) / Vagrant (2.2.1) during initial testing, where
the NIC adapters were not recognised, so I ended up using older versions using [Chocolatey](https://chocolatey.org/docs/installation#installing-chocolatey):

```powershell
choco install virtualbox --version 5.2.18
choco install vagrant --version 2.1.5
```

### Step by Step Build and Test Guide

Head over to
[https://github.com/adamrushuk/Ansible-Windows](https://github.com/adamrushuk/Ansible-Windows) and follow the step
by step [README](https://github.com/adamrushuk/Ansible-Windows/blob/master/README.md) to test it out locally using Vagrant and VirtualBox.

### Support

I'm happy to help with any questions or build errors, so please
[submit a new issue](https://github.com/adamrushuk/Ansible-Windows/issues/new) if you require any assistance.

### Credits

Although Ansible is easy to set up and configure, the simple Playbook examples and other tips were gathered from
the many excellent presentations by Trond Hindenes, and Matt Davis.
