# drakvuf-vm-save-restore
Guide for creating VM and making of restore point.



### Step : After opening disks, click on partition 4 then click additional settings and follow as shown in below images.

<img title="Image 1" alt="disks1" src="/images/disks.png" width="650" height="500">

<img title="Image 2" alt="disks2" src="/images/disks2.png" width="650" height="500">

- It will start formatting partition 4.
<img title="Image 3" alt="disks3" src="/images/disks3.png" width="650" height="500">


### Step : After successfully formatting the partition 4, open terminal and run following commands:

```
sudo pvcreate /dev/sda4
```

```
sudo vgcreate vg /dev/sda4
```

```
sudo lvcreate -L110G -n windows7-sp1 vg
```

```
sudo gedit /etc/xen/win7.cfg
```

- configure the windows.

```
arch = 'x86_64'
name = "windows7-sp1"
maxmem = 2048
memory = 2048
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
**Note -** Here, *'maxmem'*, *'memory'* and *'vcpus'* are personal preference. User can change it according to his need or system specification (Host Machine).


```
sudo xl create /etc/xen/win7.cfg
```

```
gvncviewer localhost
```

- install the windows.

<img title="Image 5" alt="windows installation" src="/images/windows.png" width="650" height="500">



### Step : Create restore point by using these commands:

```
cd drakvuf
```

```
sudo xl save domain id snapshot.sav /etc/xen/win7.cfg
```

- To restore the VM:

```
sudo xl restore /etc/xen/win7.cfg snapshot.sav
```

- To check Process list of VM:
  
```
sudo vmi-process-list windows7-sp1
```



