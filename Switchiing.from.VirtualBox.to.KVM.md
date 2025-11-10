---
undefined: ""
File: Medium (and LinkedIn)/Software/Why I ditched VirtualBox for KVM.md
sticker: lucide//file-check
---
VirtualBox is only partially "free" while KVM is really freely free 

For years I had been using VirtualBox on my desktop computers at home and at my office. I mainly needed it to run Windows inside my Linux machines, since my work sometimes requires Windows programs. I also used VirtualBox so I could play with different Linux distros from time to time. 

I generally installed the Guest Additions and the Extension Pack as well. The Extension Pack was something that I generally needed, in order to use USB drives on the host machine, and the host's NVME drives. It's also useful for users who want to use the host machine's web camera, disk encryption, and a few other functions. For years I had done so, and the main problem was that sometimes software updates broke things, due to incompatibility between VirtualBox and the Extension Pack (thanks, Oracle.) For years this was all free - VirtualBox itself, the Guest Additions, and the Extension Pack - and required no special license. 

## Until...
Then about a year ago (November 2023) I got a message from my university department about my use of VirtualBox on my office desktop, and some issue with a proprietary license. They were told by the university's IT department that an unlicensed VirtualBox box program was detected on my computer, for which the university could get into trouble with Oracle. I insisted that VirtualBox was free and required no license. After some back and forth between the department office, me, and the university's IT department, we sorted things out. VirtualBox itself was free and did not a license. But the Extension Pack required a commercial or business license. 

This seemed strange, as I had used this setup for years with no problems. After some further checking online, I found that the Extension Pack, for the longest time, required no special license, and was totally free. But at some point, apparently in 2023, Oracle suddenly changed this policy, without providing any notice to us free users. From then on, other than using a one-month trial version, purchasing a commercial or business license was required in order to run the Extension Pack. My university had no such license, so they would get in trouble if I kept using it. I was required to remove at least the Extension Pack. 


## # Goodbye, VB
Well, VirtualBox lost a user. I uninstalled the whole VirtualBox setup, since I cannot really use it without the Extension Pack. 

So if you work at a university, and if your university does not have a site license for the VirtualBox Extension Pack, I highly recommend not using VirtualBox. The same holds true if you use it for private use, say, at home. Instead, you can use KVM or other hypervisors for Linux. Though you first have to install other Linux dependencies before installing KVM, this package seems to offer a more powerful and flexible environment for virtual machines, compared to other Linux hypervisors. If you use Windows, then Windows has its own hypervisor you can install on your Windows host machine.

### Switching to KVM
I looked around for decent, free alternatives for a Linux host machine. I eventually decided to switch to the KVM-QEMU hypervisor setup for Linux, which is a bit complicated, but not too hard to install. Some other programs and dependencies have to be installed first before installing KVM, and then KVM requires a bit more setup. 

Then installing virtual machines is a bit different. It was possible to migrate my virtual machine files from VB to KVM, since KVM uses different file formats. You see, each virtual machine is basically one large data file, as far as the virtualization software and host machine are concerned. VB uses file formats like VDI, while KVM uses formats like qcow2 for virtual machines. So I had to use a separate program to convert my Windows virtual machine from VB to KVM formats. 

I'll include some instructions below on how to install KVM and convert virtual machine files to KVM format. 

## Setting up KVM 
Installing KVM is not too hard. First, you may need to check (1) if your machine hardware supports hypervisors and virtualization, and (2) if it supports KVM acceleration, if you have not run such software before. If you've already used VB on your system, you can maybe skip this check, but it doesn't hurt to do it. 

`egrep -c '(vmx|svm)' /proc/cpuinfo`
`sudo kvm-ok` 

An output of 0 for the virtualization check means that the machine does not support virtualization; otherwise, it is supported. The second command should kick out no errors. If you've not done virtualization on your machine before, you may need to reboot into the BIOS/UEFI settings, look for the motherboard's virtualization settings, enable virtualization there, reboot, and go on to install KVMin the motherboard's BIOS/UEFI settings if you've not used virtualization before. Most modern motherboards should support virtualization, so this should not be a problem. 

Next, update the system. Then install all the prerequisite programs, along with KVM. This includes the libvirt daemon and front-end KVM manager. 

`sudo apt update && sudo apt upgrade -y`

`sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y

Then oodles of text output will fly through the  terminal for a while, as the KVM system is installed. 

Next, you need to start and enable `libvirtd`, the libvirt daemon service. Then check the status to make sure that the daemon is running. 

`sudo systemctl enable --now libvirtd

`sudo systemctl status libvirtd`

Assuming you do not like living dangerously and are not managing virtual machines while logged in as root, you need to add your user to the `libvirt` and `kvm` groups. You may need to log out and log back in for these changes to take effect (a good time for a coffee break). 

`sudo usermod -aG libvirt,kvm $USER 
Or
`sudo adduser [username] libvirt

Two notes: 
1. KVM should install a network bridge automatically when it is set up, to allow guest machines to connect to the host machine and to the outside world.
2. You will need to make sure that you have lots of space in your host machine's `/var` directory in Linux, since this is where KVM prefers to put its virtual machine images. More on that later. 

### Preparing VB images for KVM 
If you have an existing VB image for a virtual machine in VDI format, a command like this should do the trick of converting it to a KVM format. Navigate to the directory where the VDI file is stored, and try the following to convert it to the default qcow2 format that KVM uses. 

 `qemu-img convert -f vdi -O qcow2 [vdi_filename].vdi [target image_name].qcow2
 
 For example: 
 `qemu-img convert -f vdi -O qcow2 Windows11.vdi Windows11.qcow2`
 
The qcow2 image should be in a size that is roughly comparable to the vdi file. 

By default, KVM wants to put new virtual machines images into a subfolder within the `var` directory, so to make things easier, this should be used as your default for importing virtual machines as well as for creating new machines. While KVM will allow you to select a different location each time you set up a VM, it can get cumbersome, and it will always default to the `var` directory when you try to import or set up a new virtual machine. So to make things easier, resign yourself to having to use this directory for your virtual machines, and make sure you have enough disk space there for all your image files. KVM defaults to the following directory:

`/var/lib/libvirt/images/

As mentioned, you'll need to make sure you have oodles of free space on your `/var` partition or directory. If you are good at hardware, it may be best to install another NVME drive (or an SSD, if you have more patience than I do), and map it as a `var` partition in you Linux `fstab. 

## Setting up or importing a virtual machine
After installing KVM, let's open the KVM program. You will be greeting with a rather spartan looking screen like this. The icon in the upper left is for importing VMs or installing new ones from scratch. 

[scr_kvm01.png]

After clicking on it, you will see several options for setting up new VMs. The most relevant are the first one, for installing a new VM from an ISO file, and the third one, for importing an existing VM. 

[scr_kvm02.png]


### Installing from scratch
If you choose the first option, you will next see a screen like this. At the bottom of this dialogue box, you will select the type of operating system to install as a VM. 

[scr_kvm03.png]

Click on the 'Browse' button on the right to find the ISO file. You will see KVM's own dialogue box like this. Its default location is the directory `/var/lib/libvirt/images`, where it will want to create the qcow2 file for the new VM. 

[scr_kvm04.png]

On the bottom of the screen, click 'Browse local' to find the ISO file on your computer. After selecting it and coming back to the dialogue box, click 'Choose volume' to continue. It will take you to a previous dialogue box (where you also identified the type of operating system), and proceed from there. From there, the rest is fairly straightforward and similar to VirtualBox and other hypervisors. You will have the options to configure the CPU and memory settings for the VM. 

### Importing a VM 
To import the qcow2 file that you converted from a previous VB image, the best way is to move the qcow2 file into the directory `/var/lib/libvirt/images`. Then go to the main startup screen, click the new machine icon, and in the dialogue box for the type of installation, click the third option to import an existing image. 

[scr_kvm02.png]

Next you will see a similar series of dialogue boxes as before to select the qcow2 file and import it. 

[scr_kvm05.png]

Again, the rest is straightforward and similar to VirtualBox, such as configuring the VM's memory and CPU settings. 

### Before firing up...
Before you actually fire up your VM, let's go back to the main screen. We see that one VM has been installed--in this case, a Windows 10 machine.

[scr_kvm06.png]

If we click on the Open icon, the second from the left, we will see the following. 

[scr_kvm08.png]

The second and third icons from the left are the most important. The third one, which looks like an arrow icon here, actually starts the VM. But first, we want to look at the second VM information icon (the blue one, as it appears in this version). This provides general settings options and configuration information about the VM, where the settings can be tweaked further if you like. For starters, you may not need to do so. At the bottom of this screen is a button that says 'Add Hardware.' 

[scr_kvm09.png]

You can click on this to add hardware options, such as a host file system that can be shared between the host and guest machines. 

[scr_kvm10.png]

That will bring up a dialogue box to configure a shared directory or file system, the source (on the host) and target (on the guest); and I think that the virtiofs driver is the most commonly used driver for this (but I could be wrong). For more about these hardware options, I would recommend googling it, as I have not yet used these very much. 

## Conclusion
Over the past year, I've been pretty happy with KVM. It is a bit more complicated to set up and run the prerequisite packages, and then the virtual machines, compared to VirtualBox. But KVM is free, and probably always will be free, with no licensing nonsense. And with KVM, updates do not break things as often as compared to my experience with VirtualBox.

So it's time to ditch VirtualBox, and switch to something more friendly, and fully free and open-source, like KVM. 



