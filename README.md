Vagrant + Ansible
--------------------

This repository contains configurations for building machine images using
[Packer](https://packer.io/).

## Requirements

The following softwares must be present on your local system before you can use __Packer__ to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

This will also require some Ansible roles installed so they can be used in the building of the VM. To install the __Anisble Roles__:

  1. Run following command in to this directory to install the roles:

    `$ ansible-galaxy install -r requirements.txt`

  2. Provisioning directory contains `playbook.yml` which is the main file for ansible to provision the server with required packages and it has defined default __variables__ to use for that roles which resides in `/provisioning/vars/main.yml`

If you don't have Ansible installed (or you're using a Windows PC?), you can simply clone the required Ansible roles from GitHub directly (use [Ansible Galaxy](https://galaxy.ansible.com/) to get the GitHub repository URLs for each role listed in `requirements.txt`), and update the `source` to match the location of the cloned roles.

## Usage

Make sure all the required software (listed above) is installed, then cd to the directory containing this README.md file, and run:

    $ packer build demo.json

After a few minutes, Packer should tell you the box was generated successfully.
