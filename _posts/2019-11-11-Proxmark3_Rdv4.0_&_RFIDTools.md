---
layout: post
title: Proxmark3 Rdv4.0 & RFIDTools
tags: proxmark3
---

Tiap kali nimbrung mengobrol soal kartu uang elektronik entah kartu bank atau kereta, saya merasa ada hutang yang belum saya lunaskan selama setahun ini. Ya...saya hampir sudah mencoba semua fungsi proxmark3, mulai dari pencarian identitas kartu tak yang tak dikenali perangkat nfc android
{% include twitter.html user="smrx86" id="1040576063351316481" %}
hingga mengintip transmisi data antara applikasi di smartphone dengan kartu nfc guna membaca saldo tersisa.
![alt text](/images/sniffing_prox3.png "proxmark3 sniffing")

Baru hari minggu ini saya mencoba menggunakan aplikasi mobile proxmark3 yang dibuat oleh [proxgrind](http://proxgrind.com/). Aplikasi ini bernama [RFIDTools](https://play.google.com/store/apps/details?id=com.rfidresearchgroup.rfidtools&hl=in). 

![alt text](/images/RFIDTools.png "proxmark3 sniffing")

Sebelumnya saya juga pernah mencoba menyambungkan aplikasi [AndProx](https://play.google.com/store/apps/details?id=au.id.micolous.andprox&hl=in), tapi tidak dapat berfungsi karna rom selain official release tidak diperbolehkan oleh aplikasi ini. Aplikasi yang sedikit mengecewakan karna rom buatan iceman dikenal memiliki banyak *tweek*.  

![alt text](/images/andprox_prox3.jpg "andprox failed")

Kompilasi dan Flashing
-

Membaca dokumentasi, perangkat proxmark3 harus diflash dengan bootloader serta rom image versi stabil yang terbaru. Dari website mereka menyediakan bootloader/romimage yang sesuai untuk [diunduh](http://proxgrind.com/free-sdk/). Untungnya repositori dari RRG di buat berdasarkan git iceman, sehingga terbuka peluang juga untuk membuild dari [source github](https://github.com/RfidResearchGroup/proxmark3) dengan beragam tweak miliknya iceman. 

> Jika anda menggunakan mac os el-capitan (seperti saya), homebrew proxmark3 sudah tidak dapat dilakukan. Paket QT5 terbaru hanya diperbolehkan dibuat dalam env minimal mac os Sierra.

Berbekal data dan pengalaman flashing sebelumnya, saya cukup yakin bisa mengkompilasi desktop tool, bootloader serta romimage yang dibutuhkan. Yup...tidak ada hambatan saat melakukan kompilasi dan coba jalankan dengan proxmark3 terhubung.

![alt text](/images/proxmark3_error_b4_flash.png "prox3 error before flash")

*Error communicate* bukan sesuatu yang patut dikhawatirkan. *Unknown command:: 0x613345d50* terjadi karna perangkat proxmark3 belum di flash dengan image yang sudah dikompilasi.

Untuk flashing dianjurkan untuk memflash bootloader terlebih dahulu, baru kemudian setelah perangkat reload dan di identifikasi lagi di *com port* di flash berbarengan dengan rom image. Setelah proses flash pertama saya mendapati perangkat dikenali sebagai */dev/cu.usbmodemicema1*. Berikut perintahnya:

```console
$ ls /dev/cu.usbmodem*
$ ./proxmark3 /dev/cu.usbmodem881 --flash --unlock-bootloader --image ../../bootrom/obj/bootrom.elf
$ ls /dev/cu.usbmodem*
$ ./proxmark3 /dev/cu.usbmodemicema1 --flash --unlock-bootloader --image ../../bootrom/obj/bootrom.elf --image ../../proxmark3/armsrc/obj/fullimage.elf

```
![alt text](/images/prox3_flashing.png "proxmark3 flashing")

Final Testing
-
Dalam test akhir saya coba menggunkan handphone yang dihubungkan via usb OTG ke proxmark3. 

![alt text](/images/IMG_0269.jpg "prox3 via otg")

Ada dua mode yang bisa digunakan, 

* Red Team Teminal yang mendukung trasmisi frekuensi tinggi dan rendah.
* Reader UI yang hanya mendukung frekuensi tinggi.

saya coba mode Red Team Terminal yang memungkin kita untuk mengetik sendiri perintah yang dikirimkan ke perangkat.
![alt text](/images/redteamterminal_prox3.jpg "redteamterminal prox3")

Extra
-
Selain bisa terhubung dengan proxmark3, applikasi RFIDTools ini ternyata juga bisa terhubung dengan ChameleonMini, usb ACR-122U serta modul PN532. Saya coba hubungkan dengan usb ACR-122U ternyata memang bisa berfungsi dengan baik.
 
![alt text](/images/IMG_0268.jpg "acr122u via otg")

![alt text](/images/nfclist_acr122.jpg "nfclist acr122u")

ref: [ RRG / Iceman repo - Proxmark3 / Proxmark / RFID / NFC](https://github.com/RfidResearchGroup/proxmark3), [Proxmark forum](http://www.proxmark.org/forum/viewtopic.php?id=6810)