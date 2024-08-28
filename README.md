# Configuring a Kiosk on a Windows Surface Tablet with Ubuntu

The following procedures and the accompanying Ansible script will configure a Windows Surface tablet with the Ubuntu OS and a configured Kiosk Mode. It includes configurations for locking down the Firefox browser to prevent user actions such as opening new browser pages, changing configurations etc. The sample configuration in the repository use [Yahoo Finance](https://finance.yahoo.com/) as the sample page but the purpose of this repository is to configure a Kiosk for single page applications.

The procedures below were tested with the following:

* Tablet: Windows Surface Pro 7 i7 16GB RAM 256GB SSD (other later versions may likely work)
* Tablet OS: Ubuntu 22.04 AMD64
* Provisioning Machine OS: Ubuntu 22.04 (Can be provision from Mac OS but the software install procedures may vary)

**Note**: These procedures assume you have some knowledge of Linux and use of the Linux terminal. 

## Installing the OS
<!--TDD Add note for referring to article -->
<!--TDD Add note to use administrator as the user name or to change it in the sc-->



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
	
4. Edit the _hosts.ini_ file located under the project root directory and add the ip address(s) of your tablets under the _[ubuntu]_ section. Each entry will be a combination of a host alias and an IP address as seen in the example below:

	```
	[ubuntu]
	my_host_one=192.168.1.40
	
	```
	If you are provisioning multiple tablets, you can add an entry for each one here. **Note**: Be sure that each entry has its own individual alias.
		
	**Note**: It is advantageous to have a static IP address for the tablet as it may change adresses after rebooting. Since this is a Kiosk, you'll have not acccess from the UI to configure the WiFi so you will have to rely on your router to tell you what address the tablet is assigned. A static address eleminates this problem as it will always have the same address. Refer to your home router's documentation on configuring a static address. 
	
5. Edit the _policies.json_ file in the project route to add restrictions to specific domains. You can add a section to the policies file for _WebsiteFilter_ to block all URL's and allow specific ones or block specific ones. The following is a configuration for _finance.yahoo.com_. It is purposly left out for this example since their website links to numerous other domains an it was easier to leave it out for our example. The following is an example of allowing only specific URL's:

	```
	"WebsiteFilter": {
	  "Block": ["<all_urls>"],
	  "Exceptions": [
	    "https://finance.yahoo.com/",
	    "http://finance.yahoo.com/",
	    "https://finance.yahoo.com/*",
	    "http://finance.yahoo.com/*"
	  ]
	}
	```
	
	It is recommended that you start without this until you have everyting configured and provisioning the tablet. For additional informaiton on configuring other policies refer to the documentation for [policy templates](https://mozilla.github.io/policy-templates/).



5. In the _root_ directory of this project, run the ansible playbook to perform the installation onto your tablet with the following command in the root directory of this project. This command will require some values to be filled in. 

Using this example as a template, replace the {username} entry with the name of the user you created while installing the OS onto the tablet and replace each of the two {password} entries with the password you assigned to the uer when you installed the OS.
	
	```
	ansible-playbook -u {username} playbook_ubuntu_firefox_frame.yml --extra-vars 'ansible_ssh_pass={password}  ansible_become_pass={password}'
	```

	

