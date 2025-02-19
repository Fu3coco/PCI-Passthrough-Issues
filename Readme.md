### Hardware
* Intel i5 14700K
* AMD Asus RX 7600 XT Dual 16GB
* MSI Z790-P motherboard


<br/>

### System
* OS - Arch
* Kernel -  Linux 6.13.2-arch1-1
* DE: KDE Plasma 6.3.1
* WM: KWin (Wayland)

<br/>
### Issue:

After using the quickpassthrough, I do get stuck after entering my password to decrypt the drive.

Note that I have also tried the above settings without diskecncryption.



<br/>

On a fersh install, the system boots and both the iGPU and the GPU are detected and both are connected to a monitor over HDMI.


<br/>



### Steps to reproduce:

Followed the steps in the [repo](https://github.com/HikariKnight/quickpassthrough):

```
git clone https://github.com/HikariKnight/quickpassthrough.git
cd quickpassthrough
go mod download
CGO_ENABLED=0 go build -ldflags="-X github.com/HikariKnight/quickpassthrough/internal/version.Version=$(git rev-parse --short HEAD)" -o quickpassthrough cmd/main.go  
``` 

`/etc/default/grub`

```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet iommu=pt intel_iommu=on"
```

Then I re-built the config file:
```
grub-mkconfig -o /boot/grub/grub.cfg
```

*System reboot*


Ran `./quickpassthrough` and made sure that the following requirements are fulfilled:

* You have already enabled IOMMU, VT-d, SVM and/or AMD-v
  inside your UEFI/BIOS advanced settings.
* Know how to edit your bootloader
* Have a bootloader timeout of at least 3 seconds to access the menu
* Enable & Configure kernel modules
* Have a backup/snapshot of your system in case the script causes your
  system to be unbootable

* You have already enabled IOMMU, VT-d, SVM and/or AMD-v
  inside your UEFI/BIOS advanced settings.
* Know how to edit your bootloader
* Have a bootloader timeout of at least 3 seconds to access the menu
* Enable & Configure kernel modules
* Have a backup/snapshot of your system in case the script causes your
  system to be unbootable


<br/>

### Additional information

`lspci -k`
```
00:02.0 Display controller: Intel Corporation Raptor Lake-S GT1 [UHD Graphics 770] (rev 04)
        DeviceName: Display controller
        Subsystem: Micro-Star International Co., Ltd. [MSI] Device 7e06
        Kernel driver in use: i915
        Kernel modules: i915, xe

03:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Navi 33 [Radeon RX 7600/7600 XT/7600M XT/7600S/7700S / PRO W7600] (rev c0)
        Subsystem: ASUSTeK Computer Inc. Device 0518
        Kernel driver in use: amdgpu
        Kernel modules: amdgpu
```


Also, for the tests when the drive has been encrypted, I've tried to include the i915 in the `/etc/mkinitpcio.conf` modules as mentioned on the [arch wiki](https://bbs.archlinux.org/viewtopic.php?pid=2070655#p2070655).

