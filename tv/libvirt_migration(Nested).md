# Libvirt live VM Migration
Panduan ini akan membantu anda melakukan migrasi VM secara live menggunakan KVM/QEMU (libvirt) didalam VM (Nested VM).

## Environment
1. OS PC1 dan PC2 menggunakan Ubuntu 18.04.xx LTS
2. Migrasi dilakukan di dalam sebuah PC yang sama (Nested VM migration)
3. Pada hypervisor yang digunakan misal VirtualBox, centang enable Nested VT-x/AMD-v
4. Insall SSH pada kedua PC (ssh-askpass)

Note: Walaupun VT-x/AMD-v tidak diaktifkan, kita masih dapat mengatur dan membuat VM dengan performance yang (mungkin) lebih lambat. Beberapa processor yang sudah diujicoba menunjukkan beberapa error ketika VT-x/AMD-v diaktifkan. 


## Step
1. Instalasi. Pastikan QEMU/KVM sudah terinstalasi dengan baik di kedua PC
2. Buat VM di salah satu PC
3. Konfigurasi Network
4. Konfigurasi Shared Storage
5. Lakukan Migrasi saat VM sedang berjalan (live).


## Instalasi QEMU/KVM
Sebelum melakukan instalasi, cek kapablitas virtualization technology (VT) pada PC
##### Install CPU Checker
```
sudo apt install cpu-checker
```

##### Cek kapabilitas VT
```
kvm-ok
```

##### Respon yang diharapkan jika VT sudah aktif

```
INFO: /dev/kvm exists
KVM acceleration can be used
```

##### Instalasi Qemu/KVM beserta dependencynya dan Virt Manager
```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
```

##### Cek status libvirt daemon
```
sudo systemctl is-active libvirtd
```
Jika status menunjukkan 'Not Connected', lakukan reboot.

##### Agar dapat membuat dan mengatur VM, masukkan user libvirt dan kvm grup
```
sudo usermod -aG libvirt $USER
```
```
sudo usermod -aG kvm $USER
```

Buatlah satu VM di salah satu PC/Host

## Konfigurasi Network
#### Buat virtual interface br0 dan assign IP ke interface br0
```
sudo nano /etc/network/interfaces
```
```
auto lo
iface lo inet loopback

auto enp0s3
iface enp0s inet manual

auto br0
iface br0 inet static
        address 192.168.18.12
        netmask 255.255.255.0
        gateway 192.168.18.1
        bridge_ports enp0s3

```
## Hubungkan Hypervisor di PC1 dengan PC2
#### File > Add Connection > centang connect to remote host
```
Method = SSH
Username = username PC2
Hostname = IP PC2
```

## Konfigurasi Shared Storage
#### Install NFS Server (PC1 dan PC2)
```
sudo apt install nfs-kernel-server
```
#### Ubah kepemilikan fileVM.qcow / fileVM.qcow2 (PC1)
```
chown nobody:nogroup /var/lib/libvirt/images/
```
#### Atur konfigurasi NFS Server (PC1)
```
sudo nano /etc/exports
```
```
/var/lib/libvirt/images *(rw,sync,no_subtree_check)
```
```
service nfs-kernel-server restart
```

#### Mount Storage di (PC2)
```
sudo mount 192.168.18.11:/var/lib/libvirt/images /var/lib/libvirt/images
```



## Migrasi Live
#### Klik kanan pada VM yang sedang berjalan > Migrate
```
Mode = Direct
Address = Alamat IP PC tujuan
Port = 49152 (default)
```

## Demo Live VM Migration (Nested VM)
```
https://youtu.be/m3B2eTfxZaY
```

#### Agar migrasi dapat dilakukan dari kedua PC, konfigurasikan langkah konfigurasi network dan konfigurasi storage di kedua PC 

Have fun !!
