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
2. **Install Sysmon:**
   - Open Command Prompt as an administrator.
   - Navigate to the directory where Sysmon was downloaded.
   - Run the command `sysmon.exe -i` to install Sysmon with default settings.
3. **Verify Sysmon Installation:**
   - In Command Prompt, run `sysmon.exe -c` to check the current configuration.
   - You should see the default configuration settings.

- **Screenshot Suggestion:** Include a screenshot of the successfully installed Sysmon on Windows 10, showing the Sysmon service running or the output of the `sysmon.exe -c` command.


### Wazuh Server
 **Description:** Installing the Wazuh server, which is a core component for security monitoring and incident response.  

**Steps:**
  1.	[Steps to install the Wazuh server]
  2.	[Configuration details for integrating with other SOC components]
•	Screenshot Suggestion: Capture the Wazuh dashboard showing successful installation and configuration.

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
