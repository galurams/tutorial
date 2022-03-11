# Libvirt live VM Migration
Panduan ini akan membantu Anda untuk membangun virtual switch dan melakukan konfigurasi port agar dapat digunakan oleh VM.

## Environment
1. PC menggunakan OS Ubuntu 18.04.06 LTS
2. 


## Langkah-langkah
Sebelum melakukan instalasi OVS, disarankan untuk melakukan instalasi net-tools untuk melakukan pengecekan interface jaringan
##### Install Net-tools
```
sudo apt install net-tools
```

##### Cek interface
```
ifconfig
```

##### Respon cek interface

```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.18.9  netmask 255.255.255.0  broadcast 192.168.18.255
        inet6 fe80::5a6e:84f9:4528:1d99  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:d2:9e:38  txqueuelen 1000  (Ethernet)
        RX packets 314038  bytes 436690037 (436.6 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 161979  bytes 15198101 (15.1 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 820  bytes 76964 (76.9 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 820  bytes 76964 (76.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

##### Instalasi OVS
```
sudo apt install openvswitch-switch
```

##### Cek apakah OVS sudah berhasil di install
```
sudo ovs-vsctl show
```

##### Respon jika OVS sudah berhasil di install
```
64db134b-b92b-4284-b6f6-ee7b7fe9216e
    ovs_version: "2.9.8"
```






Have fun !!
