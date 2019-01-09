---
title: Azure Provisioning using Ansible AWX (Tower)
description: Azure Provisioning using Ansible AWX (Tower)
categories:
  - azure
  - ansible
  - ansible-tower
  - awx
toc: true
comments: true
---

## Scenario

You've [installed and tested Ansible locally](https://adamrushuk.github.io/ansible-dsc-windows/), then
[installed Ansible AWX (Open Source Ansible Tower) using Docker](https://adamrushuk.github.io/installing-ansible-awx-docker/),
and finally [tested Ansible AWX with Windows Hosts](https://adamrushuk.github.io/testing-ansible-awx-windows-hosts/).

You now want to test Azure Provisioning using Ansible AWX.

## Solution

Building upon the work I did in previous posts, I've created a pre-configured Vagrant lab that will build a local
Ansible Control VM on your computer. You can then run a simple Ansible Playbook from the AWX Web UI to provision a
CentOS VM and all associated resources in Azure.

### Step by Step Build and Test Guide

Visit [https://github.com/adamrushuk/ansible-azure](https://github.com/adamrushuk/ansible-azure) and follow the step
by step [README](https://github.com/adamrushuk/ansible-azure/blob/master/README.md) to test Azure Provisioning
using Ansible AWX.

## Support

I'm happy to help with any questions or build errors, so please
[submit a new issue](https://github.com/adamrushuk/ansible-azure/issues/new) if you require any assistance.
