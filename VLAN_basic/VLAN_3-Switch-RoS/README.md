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

## Cara Menjalankan
1. Import konfigurasi MikroTik melalui Winbox atau `/import`
2. Load konfigurasi Ruijie melalui console
3. Jalankan DHCP client pada Edge Router (ether1)
4. Test konektivitas antar VLAN dan ke management
5. Test konektivitas antar Client/Host

## 📂 File Konfigurasi
- Edge-Router : [Config]()
- Switch 1 : [Config](VLAN_basic/VLAN_3-Switch-RoS/Mikrotik/EDGE-ROUTER_config.rsc)
- Switch 2 : [Config](VLAN_basic/VLAN_3-Switch-RoS/Mikrotik/Switch-1_config.rsc)
- Ruijie-SW 3 : [Config](VLAN_basic/VLAN_3-Switch-RoS/Mikrotik/Switch-2_config.rsc)

## Kekurangan / Kelemahan
- Saya tidak mengimplementasikan STP yang berfungsi untuk mencegah broadcast storm atau loop frame yang dikirim secara Broadcast oleh switch yang bisa menyebabkan kinerja jaringan menurun atau menyebabkan device Hang, dan saya tidak melakukan tagging pada Pure-Native path yang bisa menjadi celah keamanan untuk jaringan, yang menyebabkan serangan seperti VLAN Hopping bisa terjadi.

## ✅ Verifikasi & Pengujian

- **Management VLAN 99 reachable** dari Edge Router (ping ke Switch 1 & Switch 2 sukses, 0% loss):
  <image-card alt="Management Ping Test" src="Screenshots/management-ping-from-edge.png" ></image-card>

- **DHCP leases** di Edge Router (semua client VLAN 10 & 20 bound dengan IP benar):
  <image-card alt="DHCP Leases" src="Screenshots/dhcp-leases-print_edge.png" ></image-card>

- **RoMON neighbors** terdeteksi → semua MikroTik switch terhubung untuk manajemen Layer-2:
  <image-card alt="RoMON Neighbors in Winbox" src="Screenshots/romon-neighbors-winbox.png" ></image-card>

- **Client-side test** di VPCS (DHCP berhasil, intra-VLAN & inter-VLAN ping OK):
  <image-card alt="VPCS DHCP & Ping Test" src="Screenshots/vpcs3-dhcp-ping-test.png" ></image-card>

## Rencana perbaikan
- Saya berencana untuk mengimplementasikan protokol seperti STP,VTP,dan protokol lainnya agar Kinerja jaringan meningkat, dan berencana untuk mengimplementasikan Inter-Vlan Routing pada Switch layer 3 agar paket yang tujuannya ke jaringan internal bisa langsung di forward oleh Switch layer 3 dan Router bisa fokus pada paket yang tujuannya ke luar jaringan.
