---
layout: post
title: U-boot Environment Modification
tags:
- Openwrt
---

Dibeberapa postingan [terdahulu](https://smrx86.github.io/into_uboot/) saya tulis soal pengenalan dan penggunaan uboot saat kondisi men flash firmware.
Tadi siang saya membaca sebuah [artikel](https://medium.com/@knownsec404team/getting-started-tutorial-how-to-explore-the-camera-vulnerability-firmware-c405e25ed177) yang membahas bagaimana membypass console login via modifikasi variable environment uboot. Saya teringat rekaman [presentasi Andrew Bindner](https://www.youtube.com/watch?v=oDN7dc6cX7Y) di RSAConf2019 yang juga menggunakan metode yang sama.

Mengubah variable *init* dalam *env* bootargument bukan technique yang permanent, *env* akan kembali ke seting *default* tiap kali *reboot*. Cara ini efektif untuk menonaktifkan proteksi yang di panggil setelah linux kernel dijalankan (ex: resetting passwd).

Ada banyak variasi untuk masuk kedalam mode console bootloader, tergantung dari versi uboot digunakan. Pada router tp-link yang masih original misalnya anda harus mengetik ***'tpl'*** untuk masuk ke mode uboot **hornet**.

Dalam uji kali ini saya menggunakan tp-link mr3020 v1 ynag sudah saya ganti ubootnya menggunkan [pepe2k](https://github.com/pepe2k/u-boot_mod) sehingga untuk masuk kedalam console cukup menekan enter sesaat router dinyalakan.

![alt text](/images/modifikasi_env.png "modifikasi env")

Perintah yang saya ketikkan sebagai berikut:

```
uboot> printenv 
uboot> setenv bootargs 'console=ttyS0,115200 root=31:02 rootfstype=squashfs init=/bin/ash mtdparts=ar7240-nor0:128k(u-boot),3904(firmware),64k(ART)' 
uboot> printenv 
uboot> bootm 0x9F020000 
```

hasilnya setelah di boot akan masuk ke dalam **/bin/ash**

![alt text](/images/hasil_set_bootm.png "hasil setelah bootm")