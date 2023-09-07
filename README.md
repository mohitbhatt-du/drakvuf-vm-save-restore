# Guide For VM And Restore Point Creation.

This is a guide for creating Virtual Machine in DRAKVUF and then creating it's restore point.

***Note:*** It's only applicable if DRAKVUF is already installed.


### Step 1: To remove previous VM:
```
sudo lvremove windows7-sp1 vg
```

### Step 2: Navigate to Disks, select partition 4, access aditional settings, choose format partition as shown in below images.

<img title="Image 1" alt="Figure 1" src="/images/disks.png" width="650" height="500">

<img title="Image 2" alt="Figure 2" src="/images/disks4.png" width="650" height="500">

<img title="Image 3" alt="Figure 3" src="/images/disks2.png" width="650" height="500">



#### It will start formatting partition 4.
<img title="Image 4" alt="Figure 4" src="/images/disks3.png" width="650" height="500">


### Step 3: After successfully formatting the partition 4, open terminal and run following commands:

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

#### Configure the windows specification.

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
**Note -** Here, *'maxmem'*, *'memory'* and *'vcpus'* are personal preferences. User can change it according to his need or system specification (Host Machine).


```
sudo xl create /etc/xen/win7.cfg
```

```
gvncviewer localhost
```

#### Install and Setup the windows.

<img title="Image 5" alt="windows installation" src="/images/windows.png" width="650" height="500">



### Step 4: Creation of restore point:

```
cd drakvuf
```

```
sudo xl save domain id snapshot.sav /etc/xen/win7.cfg
```

#### To restore the VM:

```
sudo xl restore /etc/xen/win7.cfg snapshot.sav
```

#### To check process list of VM:
  
```
sudo vmi-process-list windows7-sp1
```



