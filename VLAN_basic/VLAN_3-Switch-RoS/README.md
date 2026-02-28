# VLAN_3-Switch-RoS(Router_on_Stick) (PNETLab) Version

## 📌 Deskripsi Proyek
Proyek ini merupakan simulasi jaringan yang menggunakan VLAN untuk membagi 1 Broadcast domain default pada Switch menjadi beberapa Broadcast domain untuk tujuan keamanan dan efisiensi. Proyek ini hanya membahas konsep VLAN saja(Pure VLAN).

## 🛠️ Konsep Jaringan
Dalam topologi ini, saya menerapkan:
* **Router on Stick** Jalur utama transmisi data pada switch 1 (Distributed Switch) dengan menggunakan 1 media transmisi saja untuk bertukar dan meneruskan data antar switch atau router.
* **Trunk path:** Jalur transmisi data untuk menampung frame dengan tag 802.1Q(VLAN) atau menambahkan tag pada frame, bertukar dan meneruskan frame/packet dengan menggunakan 1 media transmisi saja yang terhubung pada End System dan Intermediary Device dengan mengaktifkan fitur Trunk pada portnya untuk menerima data dan mengirim data. 
* **Access path:** Jalur transmisi data untuk client/host yang berfungsi untuk melepas tag 802.1Q(VLAN) pada frame agar client/host dapat membaca frame yang diterima.
* **Pure-Native path:** Jalur khusus transmisi data untuk device Legacy yang tidak support VLAN dengan mengirim data tanpa tagging VLAN pada frame agar device bisa membaca frame yang diterima.
* **Tagging-Native:** Jalur khusus transmisi data yang melakukan tagging pada Native untuk keamanan dan untuk management

## 🗺️ Topologi Jaringan
![Network Topology](https://github.com/fasyaAlvyan/Just_Learn_Networking/blob/main/Static-Routing_basic/cisco-redudant-static/Topology.png)

## 🗂️ IP Addressing & VLAN Scheme

| VLAN | Nama          | Subnet              | Gateway         | Keterangan                  |
|------|---------------|---------------------|-----------------|-----------------------------|
| 10   | Users-10      | 192.168.10.0/24     | 192.168.10.1    | Access VLAN (VPC3, VPC5)    |
| 20   | Users-20      | 192.168.20.0/24     | 192.168.20.1    | Access VLAN (VPC4, Linux)   |
| 99   | Management    | 10.1.99.0/29        | 10.1.99.1       | Native VLAN (SVI semua switch) |

**Management IP:**
- Edge Router     : 10.1.99.1
- Switch 1        : 10.1.99.2
- Switch 2        : 10.1.99.3
- Ruijie-SW 3     : 10.1.99.4 (rencana)

## Cara Menjalankan
1. Import konfigurasi MikroTik melalui Winbox atau `/import`
2. Load konfigurasi Ruijie melalui console
3. Jalankan DHCP client pada Edge Router (ether1)
4. Test konektivitas antar VLAN dan ke management
5. Test konektivitas antar Client/Host

## 📂 File Konfigurasi


## Kekurangan / Kelemahan
- Saya tidak mengimplementasikan STP untuk mencegah loop frame yang dikirim secara Broadcast oleh switch yang bisa menyebabkan kinerja jaringan menurun atau menyebabkan device Hank, dan saya tidak melakukan tagging pada Pure-Native path yang bisa menjadi celah keamanan untuk jaringan, yang menyebabkan serangan seperti VLAN Hopping bisa berjalan.

## Rencana perbaikan
- Saya berencana untuk mengimplementasikan protokol seperti STP,VTP,dan protokol lainnya agar Kinerja jaringan meningkat, dan berencana untuk megimplementasikan Inter-Vlan Routing pada Switch layer 3 agar paket yang tujuannya ke jaringan internal bisa langsung di forward ke tujuannya oleh Switch layer 3 dan Router bisa fokus pada paket yang tujuannya ke luar jaringan.
