+++
date = '2026-05-11T16:54:48+03:00'
draft = false
title = 'Gentoo Ansible playbook'
+++

![Gentoo Ansible](../../images/gentoo-ansible.png)

# Introduction

To ease operations when setting up Gentoo, I've written an `Ansible`
playbook that installs and configures a Gentoo system, automating away
around 30m-2h of a manual setup.

The repository can be found [here](https://github.com/sdobrau/gentoo-ansible).

# Play/role structure

The play is subdivided into multiple roles, each for a specific stage:

1. Preparing the disks: This is where partitioning and filesystems are
   prepared.
2. Grabbing the stage file: This is to have a minimal Gentoo system
   functioning for the next steps
3. Installing the base system: Essential functionality is provided
   here.
4. Configuring and installing the kernel
5. Configuring the system: essential services such as `dhcpcd` and
   `NetworkManager` 
6. Installing more tools and configuring the bootloader
7. Configuring the bootloader
8. Finalising the install
9. Ensures all the correct `USE` flags and the `world` file is set to
   install all extra packages and set up services

It is expected to be a one-off operation to fire and forget after
setting up SSH and networking. Afterwards only passwords for the root
and main user need to be manually configured.

# Consideration: Improve process with Packer and Jenkins

A webhook for this repo could be bound to a one-off script+Jenkins
pipeline which, with the help of `Packer` launches a fresh VM build
whenever any configuration state changes and is pushed to this
repo. This way I have an up-to-date golden image after each push. See
the repo at [this link](https://github.com/sdobrau/gentoo-packer) for
the `Packer` script.

# Conclusion

I enjoyed learning Ansible and hope to use it more in the future. It
definitely deserves its name of standard in configuration management.

