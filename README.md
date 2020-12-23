# CERN CentOS Installation Notes

## Making the USB Installer

Assuming the CentOS 7 ISO file has been downloaded. Follow these steps on Windows to create a bootable USB installation disk:

 1. Download and run [Rufus](https://rufus.ie/), a small tool that makes it easy to create bootable USB installation disks for various operating systems.
 2. Plug in your USB stick. I recommend a USB3 stick with a minimum 8GB capacity.
 3. Select your USB device from the `Device` dropdown menu, and select the ISO file by clicking the `SELECT` button. Make sure `Partition scheme` is set to MBR and `File system` is `Large FAT32`
 4. Click `START` and wait for the process to finish.

## Booting Into the Installer

This part is tricky as it depends on the configuration of the target machine. But the process is generally as follows:

1. Restart your machine (with the USB stick still plugged in).
2. When the computer is booting up, a message will briefly appear telling the user what key to press to enter the BIOS. Usually this is the escape key (ESC), the F1 key, or the DEL key. 
3. Press the appropriate key to enter the BIOS. You have to be quick as the computer will quickly move to boot Windows. We want to enter the BIOS to tell the computer to boot from the USB drive.
4. Look for a menu titled “Boot Menu” and enter it. You should see a list of devices connected to the system. Select the bootable USB drive you've created in the previous step.
5. Select `Install CentOS 7` from the USB disk's boot menu and hit ENTER.

## Installing CentOS

1. You will be greeted with a dialog box prompting you to select the language of installation, which is English by default. Click `Continue`.
2. Change the Date & Time settings to the correct time zone by clicking “Date & Time” and choosing the correct geographical region. Click `Done` at the top left to finish.
3. Click `Software Selection` to customize the list of installed software. Choose the `Development and Creative Workstation` option on the left side. On the right side, choose `Additional Development`, `Development Tools`, `Emacs`, `Platform Development`, `Python`, and `System Administration Tools`.
4. **(WARNING: this step is highly system-dependent and thus may not work for your system)** Click `Installation Destination`. You will see a list of local hard disks. The first disk is usually the Windows disk. Choose the second disk and make sure that the Windows disk is unselected (i.e., has no checkmark on it). Choose `Automatically configure partitioning` and click done.
5. Click `Network & Hostname` and make sure that your ethernet adapter is enabled (check the switch on the right side).
6. Click `Begin Installation`
7. While the system is being installed you'll be prompted to specify a root password and create any users. Root is a special UNIX user which has complete control over the system so make sure to assign it a password. 
8. You should also create an additional user for day-to-day running of the system. Make the user an administrator so that you can execute system-changing commands. The difference between this user and a root is that whenever you need to execute a system-modifying command, you'd need to prefix with the command `sudo` and enter the user's password. This is known as the principle of least privilege.
9. Once the installation is done, you'll be prompted to reboot the system.
10. Finally, accept the license agreement in the dialog box that shows up on first startup of CentOS.

## Installing CUDA

1. Download CUDA 11.2 local RPM installer from NVIDIA's website: `https://developer.download.nvidia.com/compute/cuda/11.2.0/local_installers/cuda-repo-rhel7-11-2-local-11.2.0_460.27.04-1.x86_64.rpm` or copy the file, if you have already downloaded it elsewhere, to your `Downloads` folder.
2. Launch the terminal: `Applications` -> `System Tools` -> `Terminal`
3. Type the following in the terminal:

Enable the Extra Packages for Enterprise Linux (EPEL) repository
`sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`

Install the CUDA repository meta-data
`sudo rpm -i cuda-repo-rhel7-11-2-local-11.2.0_460.27.04-1.x86_64.rpm`

Install CUDA
`sudo yum clean all`
`sudo yum -y install nvidia-driver-latest-dkms cuda`
`sudo yum -y install cuda-drivers`

4. Now we need to add CUDA libraries to the system path so that we can compile CUDA programs successfully. Type `nano ~/.bashrc` to open the user's `bash` configuration file. `nano` is a nice little command line based editor that is very easy to use and suitable for quick file edits.
5. Add the following lines to the end of the file:

```bash
export PATH=/usr/local/cuda-11.2/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
6. Save the file and exit `nano`.
7. Type the following in the Terminal to reload the bash configuration file.
`. ~/.bashrc`
(Yeah that's a dot at the beginning)
8. To test the CUDA installation, type the following commands:

Install CUDA sample source code into your home directory:
`cuda-install-samples-11.2.sh ~`

Compile the `nbody` example application:
```bash
cd ~/NVIDIA_CUDA-11.2_Samples/5_Simulations/nbody
make
```
Run the compiled example:
`./nbody`

## Installing CERN Software
TODO


