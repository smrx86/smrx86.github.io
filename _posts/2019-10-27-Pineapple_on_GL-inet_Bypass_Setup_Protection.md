---
layout: post
title: Pineapple on GL-inet, Bypass Setup Protection
tags:
- Openwrt
---

Sejak dari versi mk5, pengembang pineapple mulai menempatkan proteksi untuk mengamankan firmwarenya agar tak dapat [diflash keperangkat lain](https://github.com/smrx86/STPF2). Efektif dibeberapa versi, namun kadang juga tidak berguna sama sekali. Pada mk6(nano/tetra) versi 2.4.0 bahkan tidak perlu melakukan modifikasi sama sekali.

Percobaan terakhir yang pernah saya reverse adalah versi 2.4.2 dimana pada versi ini ada beberapa proteksi
 * user/password input di serial connection,
 * ssh service yang tidak aktif,
 * push button pada web setup.
Pada versi 2.6.2 saya juga menemukan proteksi serupa. Tidak berubah sama sekali.

![alt text](/images/secure_setup.png "secure setup")

Dua proteksi diawal bisa dimatikan jika sudah melewati proses setup dimana credential dikonfigurasi ulang dan setelahnya akan mengaktifkan ssh service.

![alt text](/images/setup_script.png "setup script")

Jika dilihat dari konten script setup (**/pineapple/api/Setup.php**) kita bisa menemukan bahwa pengecekan tombol dipencet atau bukan identifikasinya ada pada ada tidaknya file **/tmp/button_setup**.

Ini bisa saja diakali dengan merubah nilai variable $buttonPressed menjadi true. Tapi saya kali ini saya coba dengan menambahkan sebuah perintah di /etc/rc.local

```
#enable setup button
touch /tmp/button_setup
```
![alt text](/images/etc_mod.png "mod rc.local")

Metode bypass ini hanya bersekali sebelum adanya perintah penulisan ulang **/etc/rc.local**. 
Tapi satu kesempatan sudah cukup jika anda langsung mengeksekusinya saat status button is pressed.

{% include youtube.html id="zyRvlhZgT_s" %}