---
layout: post
title: Easy Way to Get Your Own Firmware
tags:
- openwrt
---

Ada beberapa cara untuk membuat firmware oprnwrt yang jamak dilakukan. Setidaknya ada 3 cara mudah untuk melakukannya:

1. Image builder, metode ini hampir bisa dikatakan setengah matangnya metode buildsystem. Anda tetap bisa membuat firmware dengan menggunakan scratch code yang anda sendiri. Tapi cara ini memangkas waktu untuk proses compile package yang umum sudah tersedia di repositori. [Image builder method](https://openwrt.org/docs/guide-user/additional-software/imagebuilder).
2. FMK method, cara ini seperti membongkar kompresi rootfs firmware asli, menambahkan atau memdifikasi file/package untuk kemudian menrepacknya ulang. Cara ini adalah cara yang saya gunakan di [paper](https://github.com/smrx86/STPF2/raw/master/WP_Firmware%20Hacking%2C%20Slash%20the%20Pineapple%20for%20Fun%20(en).pdf) saya dulu. Salah satu tools yang baisa digunkan adalah FMK.
3. Backup method, ini cara paling mudah untuk membuat f/w dengan cita rasa anda sendiri. Anda cukup jalankan router, tambahkan file yang akan anda inginkan, modif config yang ingin dicustom eq:firewall, dhcp config, etc. Kemudian jalankan perintah untuk mengidentifikasi lokasi f/w dan lakukan backup. 

```
root@Openwrt:~# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00020000 00010000 "u-boot"
mtd1: 00128cfc 00010000 "kernel"
mtd2: 00ea7304 00010000 "rootfs"
mtd3: 00810000 00010000 "rootfs_data"
mtd4: 00010000 00010000 "art"
mtd5: 00fd0000 00010000 "firmware" #ini lokasi f/w d mtd part

root@Openwrt:~# cat /dev/mtd5 > /tmp/backup.bin
```
Satu yang berbeda dari methode 1 & 2, jika di analyze dengan binwalk akan terlihat ada part JFFS2 dalam f/w.

```
smrx86@Manilas-MacBook ~$ binwalk backup.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
512           0x200           LZMA compressed data, properties: 0x6D, dictionary size: 8388608 bytes, uncompressed size: 3676460 bytes
1215740       0x128CFC        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 6855824 bytes, 2327 inodes, blocksize: 262144 bytes, created: 2017-01-11 06:07:39
8126464       0x7C0000        JFFS2 filesystem, big endian #ini hanya ada jika menggunakan metode backup.
```

>JFFS2 ini adalah compress dari file konten yang ditambahkan atau dimodif sebelum backup dilakukan. 

Sementara _rootfs_ yang berada di part squash filesystem sebenarnya tidak mengalami perubahan dari f/w asli yang digunakan awal oleh router.

Lalu metode apa yang sebaiknya saya pilih??? hmmm...kalau saya bilang, pilih yang paling sesuai dengan kebutuhan. Cara 1 & 2 akan butuh effort lebih, tapi akan menghasilkan f/w yang lebih bersih dan fresh ketika akan di flash.

Logika pemilihannya seperti dibawah ini:

![alt text](/images/optionbuilder.png "optionbuild") 

ref: [Image builder method](https://openwrt.org/docs/guide-user/additional-software/imagebuilder), [Slash the pineapple for fun](https://github.com/smrx86/STPF2/raw/master/WP_Firmware%20Hacking%2C%20Slash%20the%20Pineapple%20for%20Fun%20(en).pdf), [Membuat full backup image openwrt dari Router Aktif](http://inranrumani.blogspot.com/2011/10/membuat-full-backup-image-openwrt-dari.html).
