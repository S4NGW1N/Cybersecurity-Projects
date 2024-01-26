# Building Our Lab: Setting Up VirtualBox and Kali Linux

## Step 1: Download and Install VirtualBox
1. **Download VirtualBox:** Go to the [VirtualBox Downloads page](https://www.virtualbox.org/wiki/Downloads) and select the version for your operating system.
2. **Install VirtualBox:** Run the downloaded installer and follow the prompts. Accept all default settings for a standard installation.

## Step 2: Download and Set Up Kali Linux
1. **Download Kali Linux for VirtualBox:** Visit the [Kali Linux VM Images page](https://www.kali.org/get-kali/#kali-virtual-machines) and download the 64-bit version for VirtualBox.
2. **Download and Install 7Zip:** Go to [7-Zip’s download page](https://www.7-zip.org/download.html) and install 7Zip, which you'll use to extract the Kali Linux files.
3. **Extract Kali Linux Files:** Once the Kali download is complete, right-click the file and select `7Zip -> Extract Here`.
4. **Open Kali Linux VM:** Double-click the extracted `.vbox` file to open it in VirtualBox.

## Step 3: Configure Kali Linux VM
1. **Access Settings:** With the Kali VM selected in VirtualBox, click on `Settings`.
2. **Adjust Memory and Processors:**
   - Go to `System -> Motherboard` and set Base Memory to 4096 MB.
   - Go to `System -> Processor` and increase the processor count as needed.
3. **Configure Network:**
   - Go to `Network -> Adapter 2`, enable it, and set it to `Host-only Adapter`.
4. **Set Up Shared Folder:**
   - Create a folder named `Kshare` in your Downloads directory.
   - In VirtualBox settings for Kali, go to `Shared Folders`, click the folder icon with a plus sign, and set up the shared folder with the following settings:
     - Folder Path: `C:\Users\YourUsername\Downloads\Kshare`
     - Folder Name: `Kshare`
     - Mount Point: `/mnt/share`
     - Check `Auto-mount`.

## Step 4: Start and Configure Kali Linux
1. **Start Kali VM:** Click the `Start` button in VirtualBox to boot up Kali Linux.
2. **Log In:** Use the default credentials (username: `kali`, password: `kali`).
3. **Verify Shared Folder:** Open a terminal and type `cd /mnt/share` to check the shared folder.
4. **Update and Upgrade Kali:** In the terminal, run `sudo apt update` and `sudo apt upgrade`.
5. **Install VirtualBox Guest Additions:** Run `sudo apt install virtualbox-guest-x11` and restart Kali after installation.

## Step 5: Snapshot Kali VM
1. **Shut Down Kali:** Once all configurations are complete, shut down Kali.
2. **Take a Snapshot:** In VirtualBox Manager, go to `Machine -> Tools -> Snapshots` and take a snapshot for future reference.
