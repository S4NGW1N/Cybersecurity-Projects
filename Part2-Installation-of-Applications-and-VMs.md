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

### Wazuh Server
**Description:** Installing the Wazuh server using a Cloud Provider, which is a core component for security monitoring and incident response.

## Prerequisites
- An account with a Cloud Provider of your choice. In this case, we will be using Digital Ocean. If you don't have one, sign up [here](https://www.digitalocean.com/?refcode=e2ce5a05f701&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=CopyPaste).
- Basic knowledge of Linux commands.

### Steps:
#### 1. Start Droplet Creation:
- Log in to your Digital Ocean account.
- Click on the "Create" button in the top right corner, then select "Droplets" from the dropdown menu.

#### 2. Configure Droplet:
- **Region:** Choose a data center region that is closest to you for optimal performance.
- **OS Selection:** For the operating system, select Ubuntu 22.04 (LTS) x64.
- **Plan and Specs:** Select the "Basic" plan. Under "CPU Options", choose Premium Intel with NVMe SSD. Ensure your droplet has at least 8GB of RAM and 50 GB of hard drive space. This configuration is typically priced at $48/mo.
- **Authentication:** You can create a droplet using either an SSH key or a password. For simplicity, we'll use a password in this tutorial (using a password manager is recommended for security).

#### 3. Finalize and Create:
- **Hostname:** Rename your droplet to "Wazuh" or any name you prefer.
- Scroll down and click on "Create Droplet".

## Setting Up the Firewall for Wazuh Server Droplet

While waiting for your Wazuh Server droplet to be created, setting up a firewall is crucial to protect your server from unwanted external access.

### Creating a Firewall
#### 1. Navigate to Networking:
- In your Digital Ocean dashboard, find "Networking" on the left-hand side under the "Manage" dropdown menu.

#### 2. Access Firewalls:
- Click on the "Firewalls" tab, then click on "Create Firewall".

#### 3. Name Your Firewall:
- You can name your firewall anything you like. For this tutorial, we'll name it "firewall".

### Configure Firewall Rules
#### 1. Modify SSH Rules:
- By default, SSH is open to the public. We'll change this to enhance security.
- Change the type to "All TCP" and remove all IPs under sources. We want to ensure that only connections from our public IP address are allowed.
- Find your public IP address by visiting [http://whatismyipaddress.com](http://whatismyipaddress.com).
- Add your public IP address to the list of allowed sources for both TCP and UDP connections.

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

4. **Add Your Droplet to the Firewall:**
   - In the "Firewalls" settings, click the "Droplets" tab then "Add Droplets".
   - Enter the name of your Virtual Machine and click on "Add Droplet".

Now, your firewall is actively protecting your Virtual Machine.

## Accessing Your Virtual Machine

To access and manage your Wazuh server:

### Access via Droplet Console
1. **Open Droplet Console:**
   - Go to "Wazuh" (your VM Hostname) under "Droplets" and click on the "Access" tab.
   - Click "Launch Droplet Console".

### Update and Upgrade Your VM
1. **Perform System Updates:**
   - Once SSHed into your virtual machine, run:
     ```
     apt-get update && apt-get upgrade -y
     ```
   - Hit enter when prompted by any screens during the process.

### Installing Wazuh Server
1. **Run Wazuh Installer:**
   - Execute the following command to start the Wazuh installation:
     ```
     curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
     ```
     sudo tar -xvf wazuh-install-files.tar
     ```
   - Note the Username and Password provided at the end of the installation. You will need these to log into your Wazuh dashboard.

### Accessing the Wazuh Dashboard
1. **Open Wazuh Dashboard:**
   - Copy your server's public IP address (IPv4).
   - Open a new browser window and navigate to `https://<your_server_ip>`.
   - Log in using the username and password obtained during the installation.

Congratulations! You've successfully set up and accessed your Wazuh server on a Digital Ocean droplet, complete with firewall protection and system updates. Your Wazuh dashboard is now ready for use.


### The Hive Server
•	Description: Setting up The Hive server for case management in security incident response.
•	Steps:
1.	[Instructions for installing The Hive server]
2.	[Configuration steps for operational readiness]
•	Screenshot Suggestion: Show The Hive's interface with initial setup configurations.

## Tools Used
•	Windows 10
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
