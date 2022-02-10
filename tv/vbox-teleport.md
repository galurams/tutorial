# Live Teleportation/Migration VM dengan VirtualBox
Panduan ini akan membantu anda melakukan migrasi VM.

## Pre-Requisite
1. Untuk menggunakan panduan ini, anda membutuhkan dua buah PC yang terhubung pada jaringan yang sama.
2. Konfigurasi VM pada VirtualBox harus sama persis
3. VM harus mengakses storage yang sama. Artinya anda harus menyediakan shared storage, misal menggunakan samba.
4. Kedua PC sebaiknya terinstall PC dengan VirtualBox yang sama


## Konfigurasi PC
|Role|Migration Port|IP|OS|RAM|CPU|
|----|----|----|----|----|----|
|PC1|9999|192.168.0.102|Ubuntu 20.04 server|4G|2|
|PC2|9999|192.168.0.106|Ubuntu 20.04 server|16G|2|

## Konfigurasi VM
|Nama VM|Port|IP|OS|RAM|CPU|
|----|----|----|----|----|----|
|U20|9999|10.0.0.1|Ubuntu 20.04 server|1G|1|


## Skenario
VM dijalankan di PC1 lalu dimigrasi/teleport secara live dari PC1 menuju PC2 tanpa harus mematikan VM.

## Steps
1. Jalankan VM di PC1. Setelah VM berjalan sempurna, masukkan perintah untuk menerima teleportation/migration di PC2.
2. Jalankan VM di PC2
3. Masukkan perintah migrasi di PC1
4. Tunggu beberapa saat..
5. Done


## Perintah PC1
##### Syntax Migrasi
```
vboxmanage controlvm namavm teleport --host alamatIP_host_tujuan --port isi_dengan_port --password isi_password
```
##### Perintah migrasi
```
vboxmanage controlvm U20 teleport --192.168.0.106 --port 9999 --password aa
```

## Perintah PC2
##### Syntax Menerima migrasi
```
VBoxManage modifyvm namavm --teleporter on --teleporterport isi_dengan_port --teleporterpassword isi_password
```

##### Menerima migrasi
```
VBoxManage modifyvm U20 --teleporter on --teleporterport 9999 --teleporterpassword aa
```


