### Windows 10 Virtual Machine
- **Description:** This section guides you through setting up a Windows 10 virtual machine in VirtualBox. 

#### Setting Up a Windows 10 Virtual Machine
1. **Download Windows 10 ISO:**
   - Visit the [Windows 10 download page](https://www.microsoft.com/en-us/software-download/windows10) and download the Windows 10 ISO file.
       
2. **Create a New Virtual Machine in VirtualBox:**
   - Open VirtualBox and click `New` to create a new virtual machine.
   - Name the VM (e.g., "Windows 10"), select `Microsoft Windows` as the type, and `Windows 10 (64-bit)` as the version.
   - Allocate memory (RAM) to the VM. A minimum of 2048 MB is recommended.
   - Create a virtual hard disk. The default size of 50 GB should be sufficient.
   - Select the downloaded Windows 10 ISO file as the startup disk.
     
3. **Install Windows 10:**
   - Start the VM and proceed with the Windows 10 installation process.
   - Follow the on-screen instructions to complete the installation.
     
4. **Install VirtualBox Guest Additions:**
   - Once Windows 10 is installed, select `Devices` from the VM menu and click `Insert Guest Additions CD image`.
   - Run the Guest Additions installer inside the VM.
