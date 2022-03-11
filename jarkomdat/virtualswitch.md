# Virtual Switch/Virtual Port
Panduan ini akan membantu Anda untuk membangun virtual switch dan melakukan konfigurasi port agar dapat digunakan oleh host/VM.

## Environment
1. PC (OS Ubuntu 18.04.06 LTS)
2. OVS


## Steps
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

##### Cek instalasi OVS
```
sudo ovs-vsctl show
```

##### Respon jika OVS sudah berhasil di install
```
64db134b-b92b-4284-b6f6-ee7b7fe9216e
    ovs_version: "2.9.8"
```

##### Menambahkan bridge
```
sudo ovs-vsctl add-br mybridge
```

##### Mengaktifkan interface mybridge
```
sudo ifconfig mybridge up
```

##### Cek interface baru mybridge
```
ifconfig
```

##### Respon cek interface
```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.18.9  netmask 255.255.255.0  broadcast 192.168.18.255
        inet6 fe80::5a6e:84f9:4528:1d99  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:d2:9e:38  txqueuelen 1000  (Ethernet)
        RX packets 319119  bytes 443127918 (443.1 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 165145  bytes 15428457 (15.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 880  bytes 82461 (82.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 880  bytes 82461 (82.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

mybridge: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::e098:12ff:fedf:294f  prefixlen 64  scopeid 0x20<link>
        ether e2:98:12:df:29:4f  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 25  bytes 2963 (2.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

##### Attach interface aktif ke mybridge
```
sudo ovs-vsctl add-port mybridge enp0s3
```
##### Cek status port pada mybridge
```
sudo ovs-vsctl show
```
```
64db134b-b92b-4284-b6f6-ee7b7fe9216e
    Bridge mybridge
        Port "enp0s3"
            Interface "enp0s3"
        Port mybridge
            Interface mybridge
                type: internal
    ovs_version: "2.9.8"
```
##### Mencegah IP Stack mengakses internet secara langsung
```
sudo ifconfig enp0s3 0
```
##### Mengalokasikan IP Stack ke mybrige
```
sudo dhclient mybridge
```

##### Cek update interface
```
ifconfig
```
```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::5a6e:84f9:4528:1d99  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:d2:9e:38  txqueuelen 1000  (Ethernet)
        RX packets 319610  bytes 443190159 (443.1 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 165435  bytes 15451737 (15.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 1020  bytes 95611 (95.6 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1020  bytes 95611 (95.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

mybridge: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.18.9  netmask 255.255.255.0  broadcast 192.168.18.255
        inet6 fe80::e098:12ff:fedf:294f  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:d2:9e:38  txqueuelen 1000  (Ethernet)
        RX packets 304  bytes 27319 (27.3 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 61  bytes 7428 (7.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```
##### Menambahkan dua tap port (virtual port) ke mybridge dan mengaktifkannya
```
sudo ip tuntap add mode tap vport1
```
```
sudo ip tuntap add mode tap vport2
```
```
sudo ifconfig vport1 up
```
```
sudo ifconfig vport2 up
```
##### Cek vport1 dan vport2 di list interface 
```
ifconfig
```
```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.18.9  netmask 255.255.255.0  broadcast 192.168.18.255
        inet6 fe80::5a6e:84f9:4528:1d99  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:d2:9e:38  txqueuelen 1000  (Ethernet)
        RX packets 319977  bytes 443244180 (443.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 165523  bytes 15459315 (15.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 1053  bytes 98632 (98.6 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1053  bytes 98632 (98.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

mybridge: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.18.9  netmask 255.255.255.0  broadcast 192.168.18.255
        inet6 fe80::e098:12ff:fedf:294f  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:d2:9e:38  txqueuelen 1000  (Ethernet)
        RX packets 669  bytes 76110 (76.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 94  bytes 10093 (10.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vport1: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 6e:59:20:76:48:aa  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vport2: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 0a:86:0e:02:03:35  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


```

##### Tambahkan (attach) vport1 dan vport2 ke interface mybridge
```
sudo ovs-vsctl add-port mybridge vport1
```
```
sudo ovs-vsctl add-port mybridge vport2
```
##### Verifikasi vport1 dan vport2 pada interface mybridge  
```
sudo ovs-vsctl show
```
```
64db134b-b92b-4284-b6f6-ee7b7fe9216e
    Bridge mybridge
        Port "enp0s3"
            Interface "enp0s3"
        Port mybridge
            Interface mybridge
                type: internal
        Port "vport1"
            Interface "vport1"
        Port "vport2"
            Interface "vport2"
    ovs_version: "2.9.8"

```

Selanjutnya vport1 dan vport2 sudah siap digunakan sebagai interface jaringan (NIC) virtual pada host/VM.

Have fun !!
