# drakvuf-vm-save-restore
Guide for creating vm and making of restore point.

### Step 1: Open terminal and enter the command:
 `sudo nautilus`

### Step 2: Goto, other locations > computer > dev, then delete vg and mapper folder.

### Step 3: Exit from terminal and open disks from applications menu.

### Step 4: After opening disks, click on partition 4 then click additional settings and follow as shown in below images.

<img title="Image 1" alt="disks1" src="/images/disks.png">

<img title="Image 2" alt="disks2" src="/images/disks2.png">

<img title="Image 3" alt="disks3" src="/images/disks3.png">


### Step 5: If this error shows then reboot your system and do step 4 again.

<img title="Image 4" alt="Disks" src="/images/error.png">

### Step 6: After successfully formatting the partition 4, open terminal and run following commands:

`sudo pvcreate /dev/sda4`

`sudo vgcreate vg /dev/sda4`

`sudo lvcreate -L110G -n windows7-sp1 vg`

- type 'y' and hit enter for above commands.

`sudo gedit /etc/xen/win7.cfg`

- configure the windows.

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

`sudo xl create /etc/xen/win7.cfg`

`gvncviewer localhost`

- install the windows.

<img title="Image 5" alt="windows installation" src="/images/windows.png">

 - After installing the windows, again open windows config file, change the path of windows7.iso to malware.iso and boot vm again.

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
disk = [ 'phy:/dev/vg/windows7-sp1,hda,w', 'file:/home/pc-1/Downloads/malware.iso,hdc:cdrom,r' ]
```

- malware iso will be show as cd drive. copy the malware files to "E" drive

<img title="Image 6" alt="drive1" src="/images/cd-drive.png">

<img title="Image 7" alt="drive2" src="/images/copy-malware-files.png">

- Now, turn off the firewall and edit user account control from control panel as shown in below images.

<img title="Image 8" alt="firewall" src="/images/firewall.png">

<img title="Image 9" alt="user account control" src="/images/uac1.png">

### Step 7: Save the restore point by using command:

`cd drakvuf`

`sudo xl save domain id /etc/xen/win7.cfg`

- To restore the vm use this:
`sudo xl restore /etc/xen/win7.cfg`



