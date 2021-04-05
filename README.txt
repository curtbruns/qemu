# Get QEMU running on Mac with an NVMe with CMB enabled:

###########################################
# Run once
###########################################
brew install qemu
qemu-img create -f qcow2 fedora1.qcow2 20G
dd if=/dev/zero of=./nvme1.img bs=1m count=4096

###########################################
# Better way using libvirt
###########################################
virsh define ubuntu.xml
virsh start ubuntu

Start RealVNC viewer and connect to localhost -- 
NOTE: in XML, I changed port to 5902 because Apple Screen Share is on 5900, so VNC connection is to:
127.0.0.1:5902

Click "Ctrl+Alt+Del" button to reboot the machine and quickly press `Esc` to get into the boot menu
Press the number that matches the Ubuntu Server image
Install Ubuntu Server like always
Once restarted, connect to Ubunutu server:
- ssh -p 2222 user@localhost

To Shutdown image:
virsh shutdown ubuntu

To force shutdown:
virsh destroy ubuntu

# NOW TODO:
4. Run examples and connect to the emulated NVMe with CMB!


Without LIBVIRT:
Didn't get below work:

###########################################
# Run once
# Boot the Machine to Install Ubuntu
###########################################
qemu-system-x86_64 \
    -machine type=q35,accel=hvf \
    -smp 2 \
    -hda ubuntu-20.04.1-desktop-amd64.qcow2 \
    -cdrom /Users/cebruns/Downloads/ubuntu-20.04.2-live-server-amd64.iso \
    -m 6G \
    -vga virtio \
    -usb \
    -device usb-tablet \
    -display default,show-cursor=on


###########################################
# Normal Booting of the VM
###########################################
qemu-system-x86_64 \
    -machine type=q35,accel=hvf \
    -smp 2 \
    -hda ubuntu-20.04.1-desktop-amd64.qcow2 \
    -m 6G \
    -vga virtio \
    -usb \
    -device usb-tablet \
    -display default,show-cursor=on



