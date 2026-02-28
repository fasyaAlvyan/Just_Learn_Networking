# VLAN Router-on-Stick 3-Switch Lab (PNETLab)

## 📌 Deskripsi Proyek
Proyek ini merupakan simulasi jaringan **Router-on-Stick** menggunakan MikroTik dan Ruijie Switch. Tujuannya adalah memecah satu broadcast domain menjadi beberapa VLAN (10, 20, dan 99 Management) untuk meningkatkan keamanan, efisiensi, dan segmentasi jaringan.

## 🗺️ Topologi Jaringan
![Network Topology](https://github.com/fasyaAlvyan/Just_Learn_Networking/blob/main/VLAN_basic/VLAN_3-Switch-RoS/Topology.png)

## 🛠️ Konsep Jaringan
Dalam topologi ini, saya menerapkan:
* **Router on Stick** Jalur utama transmisi data pada switch 1 (Distributed Switch) dengan menggunakan 1 media transmisi saja untuk bertukar dan meneruskan data antar switch atau router.
* **Trunk path:** Jalur transmisi data untuk menampung frame dengan tag 802.1Q(VLAN) atau menambahkan tag pada frame, bertukar dan meneruskan frame/packet dengan menggunakan 1 media transmisi saja yang terhubung pada End Device dan Intermediary Device dengan mengaktifkan fitur Trunk pada portnya untuk menerima data dan mengirim data. (single link untuk multiple VLAN)
* **Access path:** Jalur transmisi data untuk client/host yang berfungsi untuk melepas tag 802.1Q(VLAN) pada frame agar client/host dapat membaca frame yang diterima. (VLAN 10 & 20)
* **Pure-Native path:** Jalur khusus transmisi data untuk device Legacy yang tidak support VLAN dengan mengirim data tanpa tagging VLAN pada frame agar device bisa membaca frame yang diterima. (Switch 2 ↔ Ruijie)
* **Native-With-Tagging:** Jalur khusus transmisi data yang melakukan tagging pada Native untuk keamanan,management dan mencegah serangan VLAN Hopping. (Switch 1 ↔ Switch 2)


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
- Edge-Router : [Config](https://github.com/fasyaAlvyan/Just_Learn_Networking/blob/main/VLAN_basic/VLAN_3-Switch-RoS/EDGE-ROUTER_config.rsc)
- Switch 1 : [Config](https://github.com/fasyaAlvyan/Just_Learn_Networking/blob/main/VLAN_basic/VLAN_3-Switch-RoS/Switch-1_config.rsc)
- Switch 2 : [Config](https://github.com/fasyaAlvyan/Just_Learn_Networking/blob/main/VLAN_basic/VLAN_3-Switch-RoS/Switch-2_config.rsc)
- Ruijie-SW 3 : [Config](https://github.com/fasyaAlvyan/Just_Learn_Networking/blob/main/VLAN_basic/VLAN_3-Switch-RoS/Ruijie-SW%203_config.txt)

## Kekurangan / Kelemahan
- Saya tidak mengimplementasikan STP yang berfungsi untuk mencegah broadcast storm atau loop frame yang dikirim secara Broadcast oleh switch yang bisa menyebabkan kinerja jaringan menurun atau menyebabkan device Hang, dan saya tidak melakukan tagging pada Pure-Native path yang bisa menjadi celah keamanan untuk jaringan, yang menyebabkan serangan seperti VLAN Hopping bisa terjadi.

## ✅ Verifikasi & Pengujian
- **Ping Test:** VPC3 (VLAN 10) dapat melakukan ping ke VPC5 (VLAN 10) dan Gateway (192.168.10.1).
- **Inter-VLAN Routing:** VPC3 (VLAN 10) dapat berkomunikasi dengan VPC4 (VLAN 20) melalui Edge-Router.
- **Management Access:** Switch 1, Switch 2, dan Ruijie dapat di-remote melalui IP 10.1.99.x dari jaringan yang diizinkan.
- **RoMON:** Semua perangkat MikroTik terlihat dalam tetangga RoMON untuk manajemen Layer 2.

## Rencana perbaikan
- Saya berencana untuk mengimplementasikan protokol seperti STP,VTP,dan protokol lainnya agar Kinerja jaringan meningkat, dan berencana untuk mengimplementasikan Inter-Vlan Routing pada Switch layer 3 agar paket yang tujuannya ke jaringan internal bisa langsung di forward oleh Switch layer 3 dan Router bisa fokus pada paket yang tujuannya ke luar jaringan.
