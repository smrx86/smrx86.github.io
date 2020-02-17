---
layout: post
title: Osmocombb, an Introduction
tags: Osmocombb
---

Osmocombb dua tahun setelah diluncurkannya OpenBSC. Pengembangan osmocombb oleh H.Welte di tujukan untuk membuat perangkat lunak yang menjalankan sisi client protokol GSM. Pada awalnya osmocombb dibuat diatas chipset *'calypso'* yang diproduksi oleh texas Instrument. Saat itu *'calypso'* mempunyai banyak informasi dataset yang bisa diperoleh dengan mudah.

Tahun 2013, ketika saya pertama kali saya mulai oprek motorola c118 sudah banyak artikel dan zine yang membahas soal osmocombb. Salah satu tulisan bahkan berikan langkah-langkah bagaimana mengubah osmocombb menjadi [BTS sederhana](http://ezine.echo.or.id/issue27/007.txt).

Pada postingan ini saya mencoba patch osmocombb oleh [Francois Louis Pönsgen](https://ntnuopen.ntnu.no/ntnu-xmlui/bitstream/handle/11250/2352780/13286_FULLTEXT.pdf?sequence=1) yang memfokuskan penggunaan osmocombb untuk menguji keamanan protokol GSM. Ada beberapa perintah extra seperti DOS, silent SMS dan test enkripsi yang bisa digunakan untuk mencek keamanan setting protokol GSM yang dijalankan sebuah BSC.

Sederhananya perangkat lunak osmocombb dijalankan dengan dua perangkat. Device *'calypso'* akan menjalankan rom *'layer 1'*, sedangkan *'layer 23, etc'* dijalankan di PC. Komunikasi antara device dengan PC dijalankan via serial port (UART).

![alt text](/images/osmocombb_communication.png "osmocombb communication")

Untuk kabel uart dapat dibuat sendiri dengan susunan pinout

| USB Uart Pinout        | Audio pinout |
| ------------- |:-------------|
| TxD | Tip |
| RxD | Ring |
| GND | Sleeve |

Pada percobaan saya menjalan 4 terminal console,

```console
terminal 1: sudo ./src/host/osmocon/osmocon -m c123xor -p /dev/ttyUSB0 target/firmware/board/compal_e88/layer1.compalram.bin
terminal 2: sudo ./src/host/layer23/src/mobile/mobile -i 127.0.0.1
terminal 3: telnet 127.0.0.1 4727
terminal 4: sudo wireshark -k -i lo -f 'port 4729'
```
![alt text](/images/four_terminal.png "four terminal continiosly")

Berikut rekaman layar ketika saya menjalankan osmocombb

{% include youtube.html id="4IWy4LOOIQY" %}
 
ref: [GSM and GPRS Security Using OsmocomBB](https://ntnuopen.ntnu.no/ntnu-xmlui/bitstream/handle/11250/2352780/13286_FULLTEXT.pdf?sequence=1)