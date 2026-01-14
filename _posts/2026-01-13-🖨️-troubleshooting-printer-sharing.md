---
layout: post
title: 🖨️ Troubleshooting Printer Sharing
author: Afarisphi
date: 2026-01-14
categories:
  - Printer
  - Sharing
  - Host
  - Client
tags:
  - Sharing
  - Host
  - Client
---
Masalah printer sharing di mana client tidak dapat mencetak sementara host bisa mencetak biasanya disebabkan oleh beberapa hal, seperti queue print error, bug Windows Update, gangguan IP jaringan, service printer bermasalah, atau pengaturan sharing dan security.

Ikuti langkah troubleshooting berikut secara bertahap:

1. Cek printer di Control Panel

   Pastikan printer sudah terpasang di client dan status printer dalam kondisi Ready (tidak Offline).
2. Cek print queue

   Buka Devices and Printers, klik kanan pada printer lalu pilih “See what’s printing”. Jika terdapat antrian print, lakukan Cancel All Documents karena queue error sering menyebabkan printer tidak bisa digunakan. Setelah itu lakukan test print melalui Printer Properties dengan menekan tombol Print Test Page.
3. Cek koneksi IP host ke client

   Lakukan ping ke IP host dari komputer client. Pastikan IP host dapat dijangkau dan tidak terjadi request timeout.
4. Restart Print Spooler

   Tekan Windows + R, ketik services.msc lalu tekan Enter. Cari service bernama Print Spooler, kemudian lakukan Restart pada service tersebut.
5. Cek fitur File and Printer Sharing / Samba

   Masuk ke menu Windows Features (Turn Windows features on or off). Pastikan fitur File and Printer Sharing atau SMB sudah aktif.
6. Akses printer melalui IP host secara langsung

   Tekan Windows + R, lalu ketik \\IP_HOST kemudian tekan Enter. Jika printer muncul, klik kanan pada printer lalu pilih Connect.
7. Restart komputer client

   Jika client masih belum bisa melakukan print, lakukan restart pada komputer client. Setelah itu ulangi pengecekan dari langkah awal.
8. Cek pengaturan sharing di host

   Apabila setelah restart client masih bermasalah, lakukan pengecekan pada komputer host. Pastikan printer tidak mengalami error dan status sharing aktif.
9. Cek pengaturan Sharing dan Security printer

   Buka Printer Properties pada komputer host. Pada tab Sharing, pastikan opsi “Share this printer” sudah dicentang. Pada tab Security, pastikan semua permission diizinkan (Allow).
10. Tukar posisi printer (opsional)

    Jika seluruh langkah sudah dilakukan namun masih bermasalah, coba pindahkan printer ke komputer lain dan jadikan sebagai host baru untuk memastikan apakah masalah berasal dari sistem atau hardware.

Kesimpulan:

Permasalahan printer sharing umumnya disebabkan oleh konfigurasi jaringan, service Windows, atau pengaturan sharing dan permission, bukan pada perangkat printer itu sendiri. Oleh karena itu, lakukan pengecekan secara berurutan agar sumber masalah dapat ditemukan dengan cepat.
