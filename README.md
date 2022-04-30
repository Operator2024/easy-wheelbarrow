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
And
```Powershell
$env:Path=$env:Path +"C:\Program Files\Oracle\VirtualBox"
$env:VAGRANT_EXPERIMENTAL="dependency_provisioners"
```

5. Use vboxmanage.exe for create interface "**vboxmanage.exe hostonlyif create**" or via GUI in VBox
6. Use vboxmanage.exe for settings created interface "**vboxmanage.exe hostonlyif ipconfig "VirtualBox Host-Only Ethernet Adapter #2" --ip 192.168.99.1 --netmask 255.255.255.252**"
7. SSH key private and public files **must be located in the root directory** and have the names '**id_rsa**' and '**id_rsa.pub**'. If you do not need access by key then comment out strings in 'Vagrantfile' from 114 to 119.
8. Vagrant up and wait...

# Warning

   * Disable Hyper-V if enable
```Powershell
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
```

# Info

Folder 'share' mount to guest system to path - /home/vagrant/shared . After provisioning can you connect to vbox via '**vagrant ssh**' or via '**ssh**'.

Default name for guest machine: Cyriaque. Attribute 'v.name' in vagrantfile

Starts vagrant with mode logging debug in to file .\vagrant.log
```powershell
 vagrant up --debug 2>&1 | Tee-Object -FilePath ".\vagrant.log"
```
