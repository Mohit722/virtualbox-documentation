# VirtualBox Disk Image Resizing Documentation

# Overview

This documentation outlines the process of resizing a VirtualBox Disk Image (VDI) file using VirtualBox 7.0.20. It provides step-by-step instructions to ensure existing data remains intact while expanding the storage capacity of your virtual machine (VM). This guide is particularly useful in scenarios where a VM has limited disk space but requires additional resources for applications such as Jenkins or JFrog Artifactory.

Yes, you can increase the size of an existing Virtual Disk Image (VDI) in VirtualBox without creating a new one. Here's how to do it:

# Precautions

While the resizing process is generally safe, it’s always a good practice to take the following precautions:

- Backup Important Data: Before making changes to disk sizes or filesystems, back up any important data to prevent accidental loss.
  
- Shut Down the VM: Ensure the VM is powered off before resizing the VDI to avoid corruption.

- Monitor the Process: If possible, monitor the resizing commands to ensure they complete successfully without errors.


# Steps to Increase the Size of an Existing VDI in VirtualBox

1. Shut Down the VM:
   - Ensure that the VM using the VDI is completely powered off. You can do this through the VirtualBox GUI or by using the command line:
     ```bash
     shutdown now
     ```
2. Open Command Line:
   - Open a terminal (Command Prompt or PowerShell on Windows) and navigate to the VirtualBox installation directory. This step may vary based on your OS:
     - Windows: Typically, `C:\Program Files\Oracle\VirtualBox\`
     - Linux: You can directly run `VBoxManage` from the terminal if it's in your PATH.
     - macOS: The `VBoxManage` command is also available from the terminal.

For windows:

1. Check VBoxManage Path: Ensure that the path to the `VBoxManage` executable is correctly set in your system's PATH environment variable. By default, `VBoxManage` is located in the VirtualBox installation directory, typically at:
   ```
   C:\Program Files\Oracle\VirtualBox\
   ```
   If it's not in your PATH, you can navigate to this directory directly in the command prompt or add it to your PATH.



# Setting Up Environment Variables

Before proceeding with the resizing process, it’s recommended to set up the environment variable for VirtualBox. This allows you to run VirtualBox commands from any command prompt without specifying the full path.

# Step 1: Set Environment Variable

1. Open System Properties:
   - Right-click on This PC or My Computer and select Properties.
   - Click on Advanced system settings on the left sidebar.
   - Click on the Environment Variables button.

2. Add New System Variable:
   - Under System variables, click on New.
   - Set the variable name as: `VIRTUALBOX_HOME`
   - Set the variable value as the path to your VirtualBox installation, typically:  C:\Program Files\Oracle\VirtualBox 
     ```
     C:\Program Files\Oracle\VirtualBox
     ```
   - Click OK to save.

3. Update the Path Variable:
   - Find the Path variable under System variables and select it.
   - Click on Edit and then New.
   - Add the following path:  
     ```
     %VIRTUALBOX_HOME%
     ```
   - Click OK to save.

# Step 2: Verify VirtualBox Installation

1. Open Command Prompt.
2. Type the following command to check the version of VirtualBox:

   ```bash
   VBoxManage --version
   ```

This command should return the version number of VirtualBox. If it doesn't, ensure that the environment variable is set correctly.


# Scenarios for Resizing VDI

1. Running Applications with Increased Resource Needs:
   - If your VM is running applications like Jenkins, which can accumulate data over time, you may need to increase the disk size to accommodate new builds or artifacts.

2. Testing Software or Development Environments:
   - Developers testing applications may require additional space for databases or code repositories, necessitating a disk size increase.

3. Improving System Performance:
   - Insufficient disk space can lead to performance degradation. Resizing the disk can help alleviate such issues.


# Steps to Resize a VDI File

# Step 1: Open Command Prompt

1. Click on the Start menu.
2. Type `cmd`, right-click on Command Prompt, and select Run as administrator.


# Step 2: Run the Resize Command

Replace "C:\Users\Admin\VirtualBox VMs\centtos\centtos.vdi" with the actual path to your VDI file and run the following command to resize the VDI to 40 GB (40960 MB):

```bash
VBoxManage modifyhd "C:\Users\Admin\VirtualBox VMs\centtos\centtos.vdi" --resize 40960
```


# Important Notes

- Existing Data: This command will not delete any existing data on the VDI.
- Ensure VM is Off: The VM using the VDI must be powered off during the resizing process.


# Step 3: Resize the Partition Inside the VM

After resizing the VDI, you may need to resize the partition to utilize the new space. This can be accomplished using `GParted`.

1. Install GParted:
   - Open your Ubuntu VM and run:
     ```bash
     sudo apt update
     sudo apt install gparted
     ```

2. Run GParted:
   - Open GParted and locate your primary partition (typically `/dev/sda1`).

3. Resize the Partition:
   - Right-click on the main partition and select Resize/Move.
   - Adjust the partition to use the newly allocated space.
   - Click Apply to finalize changes.



# Step 4: Resize the Partition inside the VM (If option 3 is not working thne)

1. Start Your VM:
   Boot up your VM in VirtualBox.

2. Resize the Filesystem:
   After resizing the VDI, you will need to resize the filesystem within the VM to utilize the newly allocated space.
   - Log in to your Ubuntu VM and open a terminal.

   - Install `cloud-guest-utils` (if it's not already installed) to use `growpart`:
     ```bash
     sudo apt update
     sudo apt install cloud-guest-utils
     ```

   - Resize the Partition: Use the following commands to resize the filesystem. Replace `/dev/sda1` with your actual partition name if it’s different:
     ```bash
     sudo growpart /dev/sda 1
     sudo resize2fs /dev/sda1
     ```
     You can find your partition layout with:
     ```bash
     lsblk
     ```

3. Verify the New Size:
   After resizing the filesystem, check the available space with:
   ```bash
   df -h
   ```


By following these steps, your VDI will be increased from 19 GB to 40 GB, and the filesystem within your Ubuntu VM will utilize the new space. 


# Conclusion

Following these steps will allow you to successfully resize your VDI file while retaining all existing data. This process is especially beneficial for VMs running resource-intensive applications, ensuring they have adequate space for growth.

# References

- [VirtualBox Documentation](https://www.virtualbox.org/manual/)
- [GParted Documentation](https://gparted.org/documentation.php)


