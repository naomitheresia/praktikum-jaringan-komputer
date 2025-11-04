# 🧩 10.4.4 Lab - Build a Switch and Router Network  

## 🔍 Deskripsi Singkat  
Modul ini bertujuan untuk membangun dan mengonfigurasi jaringan dasar yang terdiri dari satu **router (R1)**, satu **switch (S1)**, dan dua **PC (PC-A dan PC-B)**. Tujuannya adalah agar semua perangkat dapat saling berkomunikasi menggunakan **IPv4 dan IPv6**.  


## ⚙️ Langkah-Langkah Utama  

### **1. Set Up Topology and Initialize Devices**
- Rakit topologi jaringan sesuai diagram (hubungkan **R1 – S1 – PC-A – PC-B**).  
- Pastikan semua perangkat menyala dan lakukan reset konfigurasi (jika ada konfigurasi lama).  


### **2. Configure PC Hosts**
- Atur alamat IP statis pada PC-A dan PC-B sesuai tabel:  
  - **PC-A** → `192.168.1.3 /24`, Gateway: `192.168.1.1`  
  - **PC-B** → `192.168.0.3 /24`, Gateway: `192.168.0.1`  
- Uji koneksi awal dengan `ping`, yang gagal karena router belum dikonfigurasi.  


### **3. Configure the Router (R1)**
Masuk ke router menggunakan koneksi console dan lakukan konfigurasi berikut:

```bash
Router> enable
Router# config terminal
Router(config)# hostname R1
R1(config)# no ip domain-lookup
R1(config)# enable secret class
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config)# service password-encryption
R1(config)# banner motd $ Authorized Users Only! $
```

Konfigurasi antarmuka dan aktifkan:

```bash
R1(config)# interface g0/0/0
R1(config-if)# ip address 192.168.0.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown

R1(config)# interface g0/0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
```

Aktifkan routing IPv6 dan simpan konfigurasi:

```bash
R1(config)# ipv6 unicast-routing
R1# copy running-config startup-config


### **4. Configure the Switch (S1)**

Masuk ke switch dan lakukan konfigurasi dasar:

```bash
Switch> enable
Switch# config terminal
Switch(config)# hostname S1
S1(config)# no ip domain-lookup
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.1.2 255.255.255.0
S1(config-if)# no shutdown
S1(config)# ip default-gateway 192.168.1.1
S1# copy running-config startup-config
```


### **5. Verify Connectivity**
Uji konektivitas antar perangkat:
- Dari **PC-A** → `ping 192.168.0.3`
- Dari **S1** → `ping 192.168.0.3`  
Semua **ping** harus berhasil menandakan router berhasil merutekan lalu lintas antar subnet.


### **6. Display Device Information**
Gunakan perintah berikut untuk memverifikasi konfigurasi dan status jaringan:

```bash
R1# show ip route
R1# show ipv6 route
R1# show ip interface brief
R1# show ipv6 interface brief
S1# show ip interface brief
```

Perintah ini menampilkan tabel routing, status antarmuka, serta konfigurasi IPv4 dan IPv6.


## 🧠 Hasil & Pembelajaran  
- Memahami cara konfigurasi dasar **router** dan **switch** agar dua subnet dapat berkomunikasi.  
- Menguasai perintah penting **Cisco IOS** seperti `interface`, `no shutdown`, `enable secret`, dan `show ip route`.  
- Menerapkan konsep **IPv4, IPv6, default gateway**, dan **routing dasar** dalam jaringan kecil.  


## 💡 Catatan  
Jika antarmuka menunjukkan status *administratively down*, gunakan perintah berikut:
```bash
R1(config-if)# no shutdown
```


