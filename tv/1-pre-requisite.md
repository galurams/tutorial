# Pre-Requisite
Ikutilah panduan ini untuk melakukan instalasi beberapa software yang diperlukan pada OS __Ubuntu 20.04 LTS__.

Panduan ini berguna untuk mempersiapkan environment PC/Laptop anda dalam melaksanakan praktikum.

## Instalasi Git
```
sudo apt install git
```

## Instalasi Net Tools
```
sudo apt install net-tools
```

## Instalasi VirtualBox dengan Extension Packnya
```
sudo apt install virtualbox virtualbox-ext-pack
```
Saat instalasi VirtualBox, akan muncul notifikasi pop-up pada terminal. Terima semua license agreementnya. 

## Instalasi SSH
```
sudo apt install ssh
```


## Instalasi OpenvSwitch
##### Updating System
```
sudo apt-get update && apt-get upgrade -y
```
##### Install OpenvSwitch

```
sudo apt install openvswitch-switch
```
##### Jalankan OVS Daemon service (Jika Service Belum Berjalan)
```
sudo ovs-vswitchd
```
##### Verifikasi Instalasi 
```
sudo ovs-vsctl show
```
Seharusnya output terminal menujukkan versi OVS dan interface yang telah dibuat (jika ada)

Have Fun!!
