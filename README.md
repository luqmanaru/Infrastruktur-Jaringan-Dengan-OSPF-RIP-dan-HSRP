# Infrastruktur-Jaringan-Dengan-OSPF-RIP-dan-HSRP

![Contributors](https://img.shields.io/badge/contributors-1-green.svg)
![Forks](https://img.shields.io/badge/forks-0-lightgrey.svg)
![Stars](https://img.shields.io/badge/stars-0-yellow.svg)

Implementasi lengkap jaringan komputer dengan routing statis dan dinamis (OSPF, RIP), redundansi HSRP, keamanan ACL, dan NAT untuk server.

## 🌐 Topologi Jaringan

```
+----------------+      +----------------+      +----------------+
|      Switch    |      |      R1        |      |      R2        |
|                |      |                |      |                |
|  +----------+  |      |  +----------+  |      |  +----------+  |
|  |   PC1    |  |      |  |          |  |      |  |          |  |
|  +----------+  |      |  |          |  |      |  |          |  |
|  +----------+  |      |  +----------+  |      |  +----------+  |
|  |   PC2    |  |------|  | Int0/0   |------|  | Int0/0   |  |
|  +----------+  |      |  +----------+  |      |  +----------+  |
|  +----------+  |      |  | Int0/1   |  |      |  | Int0/1   |  |
|  |   PC3    |  |      |  +----------+  |      |  +----------+  |
|  +----------+  |      |                |      |                |
+----------------+      +----------------+      +----------------+
       |                        |                        |
       |                        |                        |
+----------------+      +----------------+      +----------------+
|      R3        |      |      R4        |      |    Server      |
|                |      |                |      |                |
|  +----------+  |      |  +----------+  |      |  +----------+  |
|  | Int0/0   |  |      |  | Int0/0   |  |      |  |          |  |
|  +----------+  |      |  +----------+  |      |  +----------+  |
|  | Int0/1   |------|  | Int0/1   |------|  | Int0/0   |  |
|  +----------+  |      |  +----------+  |      |  +----------+  |
|  | Int0/2   |  |      |                |      |                |
|  +----------+  |      +----------------+      +----------------+
|                |
+----------------+
```
<img width="827" height="354" alt="image" src="https://github.com/user-attachments/assets/8a0a3de5-a308-4a5d-a5af-ccc1b274ec8a" />

### Tabel Konfigurasi Perangkat

| Perangkat | Interface | IP Address | Subnet Mask | Peran |
|-----------|-----------|------------|-------------|-------|
| PC1 | - | 192.168.1.2 | 255.255.255.0 | Client |
| PC2 | - | 192.168.1.3 | 255.255.255.0 | Client |
| PC3 | - | 192.168.1.4 | 255.255.255.0 | Client |
| R1 | GigabitEthernet0/0 | 10.0.0.1 | 255.255.255.252 | Routing Static & OSPF |
| R1 | GigabitEthernet0/1 | 192.168.1.1 | 255.255.255.0 | Gateway untuk PC |
| R2 | GigabitEthernet0/0 | 10.0.0.2 | 255.255.255.252 | Routing Static & OSPF |
| R2 | GigabitEthernet0/1 | 20.0.0.1 | 255.255.255.252 | Koneksi ke R3 |
| R3 | GigabitEthernet0/0 | 20.0.0.2 | 255.255.255.252 | OSPF & RIPV2 |
| R3 | GigabitEthernet0/1 | 30.0.0.1 | 255.255.255.252 | Koneksi ke R4 |
| R3 | GigabitEthernet0/2 | 192.168.2.1 | 255.255.255.0 | Gateway untuk Switch |
| R4 | GigabitEthernet0/0 | 30.0.0.2 | 255.255.255.252 | Routing RIPV2 |
| R4 | GigabitEthernet0/1 | 40.0.0.1 | 255.255.255.252 | Koneksi ke Server |
| Server | - | 172.20.1.2 | 255.255.255.0 | Web Server |

## 📝 Penjelasan Konfigurasi

### Routing Static dan Dinamis

Proyek ini mengimplementasikan berbagai jenis routing:
- **Routing Static** pada R1 dan R2 untuk koneksi langsung
- **OSPF Area 0** pada R1 dan R2 untuk routing dinamis
- **RIPV2** pada R3 dan R4 untuk routing dinamis
- **Redistribute** untuk menghubungkan antara OSPF dan RIPV2

### Redundansi dengan HSRP

Hot Standby Router Protocol (HSRP) diimplementasikan pada Switch0 yang terhubung ke R1 dan R3 untuk menyediakan gateway redundan. Jika satu router gagal, router lainnya akan secara otomatis mengambil alih.

### Keamanan dengan ACL

Access Control List (ACL) dikonfigurasi untuk mengontrol akses dari PC1 ke server www.menit.com, memastikan hanya traffic yang diizinkan yang dapat melewati jaringan.

### Network Address Translation (NAT)

NAT diimplementasikan untuk server www.menit.com dengan IP Public 110.24.12.0/32, memungkinkan server di jaringan private untuk diakses dari internet.

---
**luqmanaru**
