---
title: "Vagrant Setup for macOS with Apple Chips"
date: 2026-03-16
draft: false
tags: ["vagrant", "macos", "apple"]
categories: ["vagrant"]
---

![Golang](/images/vagrant.webp)


Hey guys!

In this short post, we will set up a simple environment to create VMs on macOS with Apple chips (M1/M2/M3 or newer) using Vagrant.

## 1. Introduction
### What is Vagrant?

Vagrant is a tool that allows us to create and manage virtual machines easily and effectively. With it, we can create VMs from a configuration file called Vagrantfile, enabling us to share machine configurations easier, maintaining the consistency in our environments. This software has its main virtualization providers under the hood, such as VirtualBox, VMware, QEMU, Parallels, Docker, Hyper-V and others.

### Main Features
- Consistent and reproducible environments: Through the Vagrantfile configuration file, we can use the same environment for all developers involved in a project, for example.

- Reduced manual work and errors: Since the environment configurations are done through code, the manual work required to create multiple VMs is drastically reduced, it also minimize the margin for error.

## 2. Preparing the environment
### Requirements
- **OS**: macOS 11.0 (Big Sur), but it is recommended to use the most recent version of macOS (such as Monterey 12.0 or Ventura 13.0) for better compatibility and performance.
- **Processor**: Apple M1/M2/M3 or newer...
- **Package Manager**: [Homebrew](https://brew.sh/)

### Installation
First, let's install Homebrew, if you don't have it yet.
```bash
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Now, let's install [QEMU](https://www.qemu.org/), an open-source virtual machine emulator and virtualizer. You may choose another one if you prefer, like VMware, VirtualBox...
```bash
# Install QEMU
brew install qemu
```

Lastly, let's download [Vagrant](https://developer.hashicorp.com/vagrant) itself and the plugin for **QEMU**.
```bash
# Install Vagrant
brew tap hashicorp/tap
brew install hashicorp/tap/hashicorp-vagrant

# Install Vagrant plugin for QEMU
vagrant plugin install vagrant-qemu
```

## 3. VM Configuration (arm64)
Create a folder to store the Vagrant configuration file.
```bash
# Create a directory
mkdir linux-machine && cd linux-machine
```

Inside the created directory, build the machine configuration file.
```bash
# Generate a Vagrantfile
touch Vagrantfile
```

Using a text editor of choice (nano, vim, VSCode...), add the following code:
```bash
Vagrant.configure("2") do |config|
  config.vm.box = "perk/ubuntu-2204-arm64"
  config.vm.provider "qemu" do |qemu|
    # set the port of your preference
    qemu.ssh_port = "9999" 
  end
end
```
> **NOTE**: The image used was `perk/ubuntu-2204-arm64`. But you can use any other that is compatible with the QEMU provider and the arm64 architecture. You can find more images on [Vagrant Cloud](https://portal.cloud.hashicorp.com/vagrant/discover?architectures=arm64&providers=qemu). For example, the image `generic/debian12` also offers compatibility with QEMU and arm64.

## 4. Launch
Download the image and launch the VM.

```bash
# Download the image and launch
# This may take a while
vagrant up --provider=qemu
```

Access the machine’s command line via SSH.
```bash
vagrant ssh
```

`Well done, your new environment is all set up. 🙃`

Here are some Vagrant **commands** to help you out:
```bash
# See the machine status
vagrant status

# See downloaded images (box)
vagrant box list

# Remove images (box)
vagrant box remove box-name
```
