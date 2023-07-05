# drakvuf-vm-save-restore
Guide for creating vm and making of restore point.

### Step 1: Open terminal and enter the command:
 `sudo nautilus`

### Step 2: Goto, other locations > computer > dev, then delete vg and mapper folder.

### Step 3: Exit from terminal and open disks from applications menu.

### Step 4: After opening disks, click on partition 4 then click additional settings and follow as shown in below images.

<img title="Image 1" alt="Disks" src="/images/disks.png">

<img title="Image 2" alt="disks2" src="/images/disks2.png">

### Step 5: If this error shows then reboot your system and do step 4 again.

<img title="Image 3" alt="Disks" src="/images/error.png">

### Step 6: After successfully formatting the partition 4, open terminal and run following commands:

`sudo pvcreate /dev/sda4`

`sudo vgcreate vg /dev/sda4`

`sudolvcreate -L110G -n windows7-sp1 vg`

- type 'y' and hit enter for above commands.

`sudo gedit /etc/xen/win7.cfg`

- configure the vm.

```
arch = 'x86_64'
name = "windows7-sp1"
maxmem = 3000
memory = 3000
vcpus = 2
maxvcpus = 2
builder = "hvm"
boot = "cd"
hap = 1
on_poweroff = "destroy"
on_reboot = "destroy"
on_crash = "destroy"
vnc = 1
vnclisten = "0.0.0.0"
vga = "stdvga"
usb = 1
usbdevice = "tablet"
audio = 1
soundhw = "hda"
viridian = 1
altp2m = 2
shadow_memory = 32
vif = [ 'type=ioemu,model=e1000,bridge=virbr0,mac=48:9e:bd:9e:2b:0d']
disk = [ 'phy:/dev/vg/windows7-sp1,hda,w', 'file:/home/pc-1/Downloads/windows7.iso,hdc:cdrom,r' ]
```

`gvncviewer localhost`

- install the windows.

 
