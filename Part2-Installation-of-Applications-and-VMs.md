# Part 2: Installation of Applications and VMs

## Objective
In Part 2 of the SOC Automation Project, our goal is to install the necessary applications and virtual machines. By the end of this part, we will have a Windows 10 machine with Sysmon installed, a Wazuh server, and a Hive server.

## Installation Steps
### VirtualBox on Windows
 **Description:** VirtualBox is a free and open-source hosted hypervisor for x86 virtualization. It allows for the creation and management of multiple virtual machines on a single physical host, making it an essential tool for our SOC environment.  
 
If you haven't installed VirtualBox yet, check out my [Tutorial on setting up VirtualBox and Kali Linux](https://github.com/S4NGW1N/Cybersecurity-Projects/blob/Virtual-Machines-and-Lab-Setups/SettingUp-VirtualBox-KaliLinux.md).    

### Windows 10 with Sysmon
 **Description:** This section guides you through setting up a Windows 10 virtual machine in VirtualBox and installing Sysmon for enhanced security event monitoring.  
   - For a guide on installing a Windows 10 virtual machine, check out my [Tutorial on Installing Windows 10 in VirtualBox](https://github.com/S4NGW1N/Cybersecurity-Projects/blob/Virtual-Machines-and-Lab-Setups/InstallingWindows10-VM.md).

#### Installing and Configuring Sysmon
1. **Download Sysmon:**
   - Download Sysmon from the [Sysinternals Suite page](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon).
   - Download Sysmonconfig.xml from [Sysinternals Suite page](https://raw.githubusercontent.com/olafhartong/sysmon-modular/master/sysmonconfig.xml). Click Raw then Save As and save into Sysmon folder.
 
2. **Install Sysmon:**
   - Open Command Prompt as an administrator.
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/6b1df852-5ed7-490b-b672-f59e76606ad4)

   - Navigate to the directory where Sysmon was downloaded.
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/28772ceb-3aee-4933-bdfa-2091cb6fab28)
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/da5d71dd-79b5-4317-b354-f19844241a59)


   - Run the command `.\Sysmon64.exe -i .\sysmonconfig.xml` Hit enter and follow the prompts to install Sysmon with default settings.
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/98590e99-971f-4152-8c41-d715f3df4799)

3. **Verify Sysmon Installation:**
   - In Command Prompt, run `sysmon.exe -c` to check the current configuration, or in the Windows search bar type "services" and look for "Sysmon64"
   - You should see the default configuration settings.

# Wazuh Server
**Description:** Installing the Wazuh server using a Cloud Provider, which is a core component for security monitoring and incident response.

## Prerequisites
- An account with a Cloud Provider of your choice. In this case, we will be using Digital Ocean. If you don't have one, sign up [here](https://www.digitalocean.com/?refcode=e2ce5a05f701&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=CopyPaste).
- Basic knowledge of Linux commands.

### Steps:
#### 1. Start Droplet Creation:
- Log in to your Digital Ocean account.
- Click on the "Create" button in the top right corner, then select "Droplets" from the dropdown menu.
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/3e97db45-9e3a-427a-bcd7-0d6f36fca907)


#### 2. Configure Droplet:
- **Region:** Choose a data center region that is closest to you for optimal performance.
- **OS Selection:** For the operating system, select Ubuntu 22.04 (LTS) x64.
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/2f77651e-0b78-42b0-b87f-3e7530811336)

- **Plan and Specs:** Select the "Basic" plan. Under "CPU Options", choose Premium Intel with NVMe SSD. Ensure your droplet has at least 8GB of RAM and 50 GB of hard drive space. This configuration is typically priced at $48/mo.
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/11b24267-12f4-46bb-a148-fed9deea52d7)

- **Authentication:** You can create a droplet using either an SSH key or a password. For simplicity, we'll use a password in this tutorial (using a password manager is recommended for security).
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/115718e2-0710-48d8-a62b-c091c2ca7386)


#### 3. Finalize and Create:
- **Hostname:** Rename your droplet to "Wazuh" or any name you prefer.
- Scroll down and click on "Create Droplet".
![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/582983c9-666f-4020-81df-48ef6de47a03)


## Setting Up the Firewall for Wazuh Server Droplet

While waiting for your Wazuh Server droplet to be created, setting up a firewall is crucial to protect your server from unwanted external access.

### Creating a Firewall
#### 1. Navigate to Networking:
- In your Digital Ocean dashboard, find "Networking" on the left-hand side under the "Manage" dropdown menu.
- Click on the "Firewalls" tab, then click on "Create Firewall".
![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/40a41b68-dc49-45ec-baf7-6e972193cb8c)

#### 2. Name Your Firewall:
- You can name your firewall anything you like. For this tutorial, we'll name it "firewall".

### Configure Firewall Rules
#### 1. Modify SSH Rules:
- By default, SSH is open to the public. We'll change this to enhance security.
- Change the type to "All TCP" and remove all IPs under sources. We want to ensure that only connections from our public IP address are allowed.
- Find your public IP address by visiting [http://whatismyipaddress.com](http://whatismyipaddress.com).
- Add your public IP address to the list of allowed sources for both TCP and UDP connections.
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/48936686-3dbe-4d91-9af1-002adb5c7a91)


#### 2. Complete the Firewall Setup:
- After configuring the rules, scroll down and click "Create Firewall".

### Assigning the Firewall to Your Droplet
- Once the firewall is set up, you can add your Wazuh Server droplet to this firewall. This step is crucial for ensuring that SSH access, and potentially other services, are secured and only accessible from your specified IP address, protecting against unauthorized access attempts.


## Adding Your Droplet to the Firewall
After setting up your firewall, the next step is to ensure it's protecting your Wazuh Server Virtual Machine (VM).

1. **Navigate to Droplets:**
   - In the Digital Ocean dashboard, click on "Droplets" located on the left-hand side under the "Manage" dropdown.

2. **Access Your Wazuh Server:**
   - Find and click on your Wazuh server droplet named "Wazuh".

3. **Edit Firewall Settings:**
   - Select "Networking" and scroll down to "Firewalls".
   - Click on "Edit" next to the firewall you created.
![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/6a955598-33bf-4b07-88e2-aaea5244f95f)


4. **Add Your Droplet to the Firewall:**
   - In the "Firewalls" settings, click the "Droplets" tab then "Add Droplets".
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/1e2c1286-61d3-4876-b127-61a443e41132)
   - Enter the name of your Virtual Machine and click on "Add Droplet".
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/51703cf0-b7de-44b2-a631-79a63d511a1d)

Now, your firewall is actively protecting your Virtual Machine.

## Accessing Your Virtual Machine

To access and manage your Wazuh server:

### Access via Droplet Console
1. **Open Droplet Console:**
   - Go to "Wazuh" (your VM Hostname) under "Droplets" and click on the "Access" tab.
   - Click "Launch Droplet Console".
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/31ed4c4a-535f-4b3f-aa54-b6eddb6797d6)


### Update and Upgrade Your VM
1. **Perform System Updates:**
   - Once SSHed into your virtual machine, run:
     ```
     apt-get update && apt-get upgrade -y
     ```
   - Hit enter when prompted by any screens during the process.
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/d100b169-b373-42d7-9113-1d0aa120c0f4)
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/1db88c18-78b3-4cd1-8f9a-30472921c9ab)


### Installing Wazuh Server
1. **Run Wazuh Installer:**
   - Execute the following command to start the Wazuh installation:
     ```
     curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
     ```
     ```
     sudo tar -xvf wazuh-install-files.tar
     ```
   - Note the Username and Password provided at the end of the installation. You will need these to log into your Wazuh dashboard.
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/d0cd6ff3-f608-441c-98ad-d35618ec18df)


### Accessing the Wazuh Dashboard
1. **Open Wazuh Dashboard:**
   - Copy your server's public IP address (IPv4).
   - Open a new browser window and navigate to `https://<your_server_ip>`.
   - Log in using the username and password obtained during the installation.
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/347355a4-187f-4d9d-8ae5-1b12f6503f34)
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/901bd8e7-de23-4f5a-8491-5a853a0d5bad)
     ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/82e52beb-3de9-4985-b43b-f8b186285dcf)

#### Congratulations! You've successfully set up and accessed your Wazuh server on a Digital Ocean droplet, complete with firewall protection and system updates. Your Wazuh dashboard is now ready for use.



# The Hive Server

### Steps to Install The Hive Server on Digital Ocean

### 1. Start Droplet Creation for The Hive:
- Log into your Digital Ocean account.
- Click on the "Create" button in the top right corner, then select "Droplets" from the dropdown menu.

### 2. Configure The Hive Droplet:
- **Region:** Choose the same data center region used for your Wazuh server for optimal performance and reduced latency.
- **OS Selection:** Select Ubuntu 22.04 (LTS) x64 as the operating system.
- **Plan and Specs:** Choose the "Basic" plan. Under "CPU Options", select Premium Intel with NVMe SSD. Ensure the droplet has at least 8GB of RAM and 50 GB of hard drive space, similar to the Wazuh server setup. This configuration is typically priced at $48/mo.
- **Authentication:** Use the same method (SSH key or password) as your Wazuh server for consistency. Using an SSH key is recommended for enhanced security.
- **Hostname:** Name your droplet "The Hive".

### 3. Finalize and Create The Hive Droplet:
- Ensure the firewall you previously created for the Wazuh server is selected to be applied to "The Hive" droplet.
- Scroll down and click on "Create Droplet".
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/bbbe7fff-b936-4c77-b404-d3016ad93459)


### Assigning the Firewall to The Hive Droplet:
- Since the firewall setup was completed during the Wazuh server installation, ensure "The Hive" droplet is added to the same firewall to maintain a consistent security posture.
  ![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/6f10a008-4cf9-4872-b580-9fa164dff133)

# The Hive Installation Guide

This guide provides a comprehensive walkthrough for installing The Hive, including the necessary prerequisites such as Java, Cassandra, Elasticsearch, and The Hive itself.

## Getting Started

Ensure you have access to your server. Launch your Droplet console to SSH in. If you encounter issues, use the Launch Recovery Console. The default username is `root`, and the password is what you set during the droplet setup. If necessary, you can reset the password using the "Reset Root Password" button.

## Installing Prerequisites

Before installing Java, several packages need to be installed:

```bash
apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl software-properties-common python3-pip lsb-release
```
Enter Y for yes. 

![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/f2523924-c02d-4837-a4d2-164714c6b008)

When you encounter the purple screen hit enter.
![image](https://github.com/S4NGW1N/Cybersecurity-Projects/assets/150701059/1ad575d6-a73a-46d9-9bb5-d15308e549b3)

**OPTIONAL/Quick Fix**
- If the "wget" command doesn't work for Java then the URL might be either incorrect or outdated or wget hasn't been installed.
Try the following commands for a quick fix:
```
sudo apt update
```
```
sudo apt install wget
```
**Side Note**
- enter commands one by one. Copying and pasting it all at once can cause shell to be confused by lack of line breaks/proper command termination.
  
### Installing Java
 Java is essential for running The Hive. Install it using the following commands:
```
wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
```
```
sudo apt update
```
```
sudo apt install java-common java-11-amazon-corretto-jdk
```
```
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment 
```
```
export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"
```
### Installing Cassandra
Cassandra is the next component to install:
```
wget -qO - https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor -o /usr/share/keyrings/cassandra-archive.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
```
```
sudo apt update
```
```
sudo apt install cassandra
```
**Optional: Configuring Elastic Search**
- If you choose to install Elastic Search, first configure JVM Options: 
  Create a jvm.options file under /etc/elasticsearch/jvm.options.d
```
echo "-Dlog4j2.formatMsgNoLookups=true\n-Xms2g\n-Xmx2g" | sudo tee /etc/elasticsearch/jvm.options.d/jvm.options
```
### Installing Elastic Search
Proceed with Installing Elastic Search:
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
```
sudo apt-get install apt-transport-https
```
```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```
```
sudo apt update
```
```
sudo apt install elasticsearch
```
### Installing The Hive
Finally, Install The Hive:
```
wget -O- https://archives.strangebee.com/keys/strangebee.gpg | sudo gpg --dearmor -o /usr/share/keyrings/strangebee-archive-keyring.gpg
```
```
echo 'deb [signed-by=/usr/share/keyrings/strangebee-archive-keyring.gpg] https://deb.strangebee.com thehive-5.2 main' | sudo tee -a /etc/apt/sources.list.d/strangebee.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y thehive
```

### Accessing The Hive
After installation, The Hive is accessible on port 9000. The default credentials are "admin@thehive.local" with the password "secret"
Congratulations! You have successfully installed The Hive along with all the necessary components on your server.

## Tools Used
•	Windows 10
•	Cloud Provider (Digital Ocean)
•	Sysmon
•	Wazuh
•	The Hive
•	VirtualBox (for VM creation)


## Key Takeaways
•	Setup and Configuration: Understanding the installation and configuration process of each tool.
•	Integration: Learning how these tools integrate and work together in a SOC environment.
•	Practical Skills: Gaining hands-on experience in setting up a functional SOC lab.

## Conclusion
This part of the project is crucial in building the foundation of our SOC Homelab. The successful installation and configuration of these tools set the stage for the operational aspects of security monitoring and incident response.
