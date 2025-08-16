# LAB-23-Firewall-chain-Forward
tanggal 16 Agustus 

![m]()

Dri gambar topologinya menunjukkan bahwa **PC dan Laptop hanya boleh mengakses situs [http://lms.rosctock.net](http://lms.rosctock.net)**. Semua akses lain ke internet akan diblokir menggunakan **chain forward** di Mikrotik.

---

# Langkah Konfigurasi Firewall Chain Forward di Mikrotik

1. Tambahkan Address List untuk Situs yang Diizinkan

Kita harus menambahkan alamat IP atau domain situs `lms.rosctock.net` ke **address-list**:

```bash
/ip firewall address-list add list=allow-sites address=lms.rosctock.net
```

*Jika domain tidak bisa langsung di-resolve, bisa ping dulu untuk tahu IP-nya lalu masukkan IP tersebut*

![m]()

---

2. Izinkan Akses ke Situs yang Diizinkan

```bash
/ip firewall filter add chain=forward dst-address-list=allow-sites action=accept comment="Allow access to lms.rosctock.net"
```

---

3. Blokir Semua Akses Internet Lain

```bash
/ip firewall filter add chain=forward action=drop comment="Block all other internet access"
```

---
catatan urutan Rule:  
Pastikan rule **Allow** ditempatkan di atas rule **Block**, karena Mikrotik akan membaca aturan firewall dari atas ke bawah.

---

**pengujian**

* Saat client mengakses `lms.rosctock.net` → berhasil terbuka.
* Saat client mencoba membuka situs lain → akses ditolak (request timeout).

# Kesimpulan

Dengan **chain forward**, kita dapat mengontrol lalu lintas yang melewati router.  
selain ke situs yang diizinkan akan diblokir, sehingga jaringan menjadi lebih aman dan penggunaan internet lebih terkontrol.
