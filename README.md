# Learn Ansible!

### For Windows Users

The easiest and most obvious deployment option is to run on our PC! However Windows doesn't play nice with Ansible due to some Python tooling errors. So instead, we'll setup WSL, Ubuntu on Windows, and go from there. Hope you like Linux!

In terminal run `wsl --install`

#

**Getting back into WSL**

If you end up closing your terminal and can't get back into your WSL instance then see below:

`wsl --list` will show all wsl instances on your PC.

`wsl -d Ubuntu` will start wsl for that specific instance and distro, in this case this is our Ansible distro. 

#

### Ubuntu Linux

#### Getting Started
Enter your username and password

Then `sudo apt update` and `sudo agt upgrade` to update (and upgrade) the machine.

Then we will start installing packages needed for ansible:

    sudo apt install python-is-python3
    sudo apt install pip
    sudo apt install ansible
    sudo apt install nano

*You can also install **Vim** over **Nano**, but if you have zero preference I find Nano easier, while Vim is more industry dominant.*

#

### Ansible Quickstart

Then create a project folder for Ansible using `mkdir ansible_quickstart` and `cd ansible_quickstart` to enter the folder. 

*docs.ansible reccomends a single directory structure so it's easy to apply source control with tools like Git.*

*If you're not use to Linux and ever lost use `ls` to list out all the files in your current directory, and then use `cd filename` to access that file. Also `cd ..` will move you up and out of the directory you're in.* 


### Ansible-Galaxy Collections

Before we get started with inventories and commands we needs collections to support the enviorment. With normal Python capable machines Ansible works quite well out of the box, but network devices can have limitations that can be an obstacle. 

    ansible-galaxy collection install ansible.netcommon cisco.ios paloaltonetworks.panos

*You can also find collections at https://galaxy.ansible.com/ui/ and then only grab collections from the official vendor or the official Ansible community.*
#

Then we can start working on a device inventory. Don't panic! We'll start with just one device for testing. Create inventory.yaml using the command `touch inventory.yaml`.

*You can also create an INI file with different formatting, but for our use case YAML scales better.*

Now we get to use our designated text editor, this guide will use Nano, run `nano inventory.yaml`

Paste. or type, something similar to below but with a ready test device.

myhosts:
    hosts:
        my_host_01:
            ansible_host: 192.168.10.1

When you're done `ctrl x` and `y` and `enter` to quit and save. 

*Double check the host is reachable with the ping command.*

Then verify the list with `ansible-inventory -i inventory.ini --list`
And ping the myhosts group with: 

    ansible myhosts -m ping -i inventory.yaml -k \
    -e 'ansible_connection=network_cli ansible_network_os=ios'

*Remove `-k` if no ssh password is required.*

*This is bit unique, normally we could get away with `ansible myhosts -m ping -i inventory.yaml -k`, but because this isn't a full Python interpreter host it won't process Python's built in Ping function. Instead it needs a special config to define what device the host is to Ansible so it can change how it asks for a ping response from the device. That's this portion `-e 'ansible_connection=network_cli ansible_network_os=ios'`* 

Now we have a working inventory that's been tested and validated!

#

A common practice in Ansible testing and training is to begin building Intent or IaC using Ansible to poll, pull, and save configurations in the field. 

Let's get a small pool of 5 test devices. 

3 Switches
2 Firewalls
