# How to use

1. Install [**VBox above 6**](https://www.virtualbox.org/wiki/Downloads) version
2. Install [**VBox extension**](https://download.virtualbox.org/virtualbox/6.1.34/Oracle_VM_VirtualBox_Extension_Pack-6.1.34.vbox-extpack)
3. Install [**Vagrant above 2.2.6**](https://www.vagrantup.com/downloads) version and **reboot**

### FOR RUSSIAN WITHOUT VPN

**Folder mega.nz with latest versions and checksums - [click here](https://mega.nz/folder/cwUVjDAB#2w4jWxsmvHM4MOfgNBfMKw)**


Example with structure:
```
 root
  - Oracle_VM_VirtualBox_Extension
  - VirtualBox-6.1.exe
  - Vagrant_2.2.19.msi
  - etc..
```
4. Open powershell as **administrator** in the root deirectory
```Powershell
.\VirtualBox-6.1.exe
.\Oracle_VM_VirtualBox_Extension
.\Vagrant_2.2.19.msi
```
And execute powershell script

```Powershell
$env:Path=$env:Path +";C:\Program Files\Oracle\VirtualBox"
$env:VAGRANT_EXPERIMENTAL="dependency_provisioners"
```
>These environment variables have been changed for the duration of the Powershell session


5. Use vboxmanage.exe for create interface "**vboxmanage.exe hostonlyif create**" or via GUI in VBox
   
```
vboxmanage hostonlyif create
# Interface '%random_interface_name%' was successfully created
```

6. Use vboxmanage.exe for settings created interface **`"vboxmanage.exe hostonlyif ipconfig "%random_interface_name%" --ip 192.168.99.1 --netmask 255.255.255.252"`** and add **`%random_interface_name%`** of created interface to [**Vagrantfile**](https://github.com/Operator2024/easy-wheelbarrow/blob/master/Vagrantfile#L138) 

```
vboxmanage list hostonlyifs

Name:            VirtualBox Host-Only Ethernet Adapter
GUID:            4a7fa59f-50a0-4e44-a91a-dc0064592f8a
DHCP:            Disabled
IPAddress:       192.168.56.1
NetworkMask:     255.255.255.0
IPV6Address:
IPV6NetworkMaskPrefixLength: 0
HardwareAddress: 0a:00:27:00:00:0b
MediumType:      Ethernet
Wireless:        No
Status:          Up
VBoxNetworkName: HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter

Name:            VirtualBox Host-Only Ethernet Adapter #2
GUID:            695a2c2b-7783-462f-b817-adb8693970aa
DHCP:            Disabled
IPAddress:       192.168.99.1
NetworkMask:     255.255.255.252
IPV6Address:
IPV6NetworkMaskPrefixLength: 0
HardwareAddress: 0a:00:27:00:00:0f
MediumType:      Ethernet
Wireless:        No
Status:          Up
VBoxNetworkName: HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter #2
```

7. SSH key private and public files **must be located in the root directory** and have the names '**id_rsa**' and '**id_rsa.pub**'. If you do not need access by key then comment out strings in 'Vagrantfile' from 118 to 122.
8. if `https://vagrantcloud.com/search.` is not available (in Russia, for example), then need comment out 'config.vm.box_version' and set 'config.vm.box_url'. For example, ubuntu cloud image - `https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64-vagrant.box`. Checksums for the image are also located on the site where the image is stored
9. Vagrant up and wait...

# Warning

   * Disable Hyper-V if enable
```Powershell
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
```

# Info

> ⚠️ Only the first "**vagrant up**" command produces setup and provisioning VM, subsequent runs '**vagrant up**' produce only run of the VM.
However, if executing '**vagrant up provision**' then will be produced step "**OS updater**" from [**Vagrantfile**](https://github.com/Operator2024/easy-wheelbarrow/blob/master/Vagrantfile#L104) 

1. **vagrant box list** - display list of boxes
2. **vagrant up** - running the machine
3. **vagrant halt** - shutdown the machine
4. **vagrant destroy** - full remove the machine

Folder '**share**' mount to guest system to path: `/home/vagrant/shared`. After provisioning can you connect to vbox via '**vagrant ssh**' or via '**ssh**'.

Default name for guest machine: **Cyriaque**. Attribute 'v.name' in Vagrantfile

Starts vagrant with mode logging debug in to file .\vagrant.log
```powershell
 vagrant up --debug 2>&1 | Tee-Object -FilePath ".\vagrant.log"
```
