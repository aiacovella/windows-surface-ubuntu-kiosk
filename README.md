# Configuring a Kiosk on a Windows Surface Tablet with Ubuntu

The following procedures and the accompanying Ansible script will configure a Windows Surface tablet with the Ubuntu OS and a configured Kiosk Mode. It includes configurations for locking down the Firefox browser to prevent user actions such as opening new browser pages, changing configurations etc. The sample configuration in the repository use [Yahoo Finance](https://finance.yahoo.com/) as the sample page but the purpose of this repository is to configure a Kiosk for single page applications.

The procedures below were tested with the following:

* Tablet: Windows Surface Pro 7 i7 16GB RAM 256GB SSD (other later versions may likely work)
* Tablet OS: Ubuntu 22.04 AMD64
* Provisioning Machine OS: Ubuntu 22.04 (Can be provision from Mac OS but the software install procedures may vary)

**Note**: These procedures assume you have some knowledge of Linux and use of the Linux terminal. 

## Installing the OS


## Installing the software on the provisioning
This is the software required to run the Ansible script. The procedures for installation are specific to Ubuntu but the scripts will run on OSX if the same software is installed.


1. **Install the Git client** 

	Execute the following commands in the terminal:
	
	```
	sudo apt update
	sudo apt install git
	```
	
	If you have trouble install this, see this [article](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu) for installing the Git client.

2. **Install the Ansible client**

	Execute the following commands in the terminal:

	```
	sudo apt update
	sudo apt install software-properties-common
	sudo add-apt-repository --yes --update ppa:ansible/ansible
	sudo apt install ansible
	```
3. Check out the source from the repository by executing the following commands in the terminal:

	```
	cd ~
	git clone https://github.com/aiacovella/windows-tablet-ubuntu-kiosk.git
	```
	 If you followed the directions above, you should have a directory named *_windows-tablet-ubuntu-kiosk_* under your home directory. This will be referred to as the project root going forward. 
	 
4. Initialize the provisioning by executing the following: 

	```
	cd ~/windows-tablet-ubuntu-kiosk
	ansible-playbook playbook_ubuntu_firefox_frame.yml
	```
	

