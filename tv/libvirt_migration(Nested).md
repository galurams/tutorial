# Libvirt live VM Migration
Panduan ini akan membantu anda melakukan migrasi VM secara live menggunakan KVM/QEMU (libvirt) didalam VM (Nested VM).

## Environment
1. OS PC1 menggunakan Ubuntu 20.04.xx LTS, OS PC2 menggunakan Ubuntu 18.04.xx LTS
2. Migrasi dilakukan di dalam sebuah PC yang sama, maka konfigurasi shared storage tidak perlu dilakukan
3. Pada hypervisor yang digunakan misal VirtualBox, enable Nested VT-x/AMD-v
4. 


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

##### Aktifkan libvirt daemon
```
sudo systemctl is-active libvirtd
```

##### Agar dapat membuat dan mengatur VM, masukkan user libvirt dan kvm grup
```
sudo usermod -aG libvirt $USER
```
```
sudo usermod -aG kvm $USER
```

