# Membuat Virtual Network Menggunakan OpenvSwitch
Panduan ini digunakan untuk membangun interface virtual yang akan digunakan oleh virtual machine. Pastikan komputer memiliki hubungan internet selama melakukan praktikum ini.

## Konfigurasi Pada OpenvSwitch
##### Menambahkan interface bridge
```
sudo ovs-vsctl add-br sistel
```
sistel adalah nama interface bridge yang digunakan
##### Verifikasi bridge yang telah dibuat
```
sudo ovs-vsctl show
```
Seharusnya sudah ada interface bridge baru sistel. Interface sudah tersedia, namun belum dijalankan. 
##### Menjalankan interface sistel
```
sudo ifconfig sistel up
```
##### Verifikasi interface sistel
```
ifconfig
```
Seharusnya output interface pada terminal sudah menambahkan interface baru dengan nama sistel

##### Menambahkan interface fisik ke interface virtual bridge
```
sudo ovs-vsctl add-port sistel wlp4s0
```
wlp4s0 adalah interface wifi. Interface ini digunakan karena saat ini hubungan internet didapat dari interface fisik tersebut. Jika hubungan internet didapat dari interface lain, maka lakukan penyesuaian.

##### Verifikasi interface fisik pada interface virtual bridge
```
sudo ovs-vsctl show
```
Seharusnya port dan interface port sudah berubah sesuai dengan yang diinputkan.

##### Lakukan ping misal ke google.com
```
ping google.com
```
Ping tidak akan berhasil karena IP Stack belum di assign ke virtual bridge

#####  Hapus IP dari wlp4s0 
```
ifconfig wlp4s0 0
```
##### Jalankan dhclient pada interface bridge
```
dhclient sistel
```
##### Verifikasi kembali IP interface sistel
```
ifconfig
```
Interface sistel seharusnya sudah memiliki alamat IP. Ping bisa di coba kembali.
##### Verifikasi koneksi internet
```
ping google.com
```
Seharusnya ping sudah dapat dilakukan
##### Untuk memeriksa jalur routing pada PC anda saat ini gunakan perintah:
```
route -n
```
##### Menambahkan interface tap
```
ip tuntap add mode tap vport1
```
Perintah tersebut akan menambahkan virtual port pada bridge sistel dengan nama vport1. Buatlah virtual port yang kedua dengan nama vport2
##### Menjalankan tap vport1
```
ifconfig vport1 up
```
Perintah tersebut hanya menjalankan vport1 saja.
##### Verifikasi interface tap
```
ifconfig
```
##### Menambahkan tap port pada virtual bridge
```
sudo ovs-vsctl add-port sistel vport1
```
Perintah tersebut hanya menambahkan vport1 saja pada virtual bridge. Tambahkan kedua vport pada virtual bridge.
##### Verifikasi interface tap sudah terdeteksi pada interface virtual bridge
```
sudo ovs-vsctl show
```

Tap port sudah dapat digunakan sebagai interface pada mesin hypervisor. Silahkan coba untuk menggunakan vport1 dan vport2 yang telah dibuat.


##### Untuk menghapus interface
```
sudo ovs-vsctl del-br sistel
```
Menghapus interface diperlukan jika terjadi kegagalan pada proses pembuatan virtual network.

Have Fun!!
