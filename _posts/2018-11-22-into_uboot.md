---
layout: post
title: Into Uboot
---

tl:dr Posting ini akan membahas sedikit tentang bootloader. Sample yang digunakan di labs ini adalah uboot.

Kalau anda baca [post sebelumnya][1], saya sempat tuliskan perihal __proses sela__ yang biasanya dipergunakan untuk memflash ulang nand memory sebuah router.

Bawaan router glinet sudah menggunakan bootloader mod dari [pepe2k][2] yang memungkinkan proses debrick lewat http. tapi disini kita coba lihat apa saja sebenarnya proses dibelakangnya.

Ada tiga opsi lanjutan untuk masuk kedalam console uboot. Ketiga nya hanya soal berapa lama kita menahan tombol reset saat router dinyalakan.

```
*****************************************
*      U-Boot 1.1.4  (Jun 25 2014)      *
*****************************************

AP121 (AR9331) U-Boot for GL-iNet

DRAM:  64 MB
FLASH: Winbond W25Q128 (16 MB)

LED on during eth initialization...

Press reset button for at least:
- 3 sec. to run web failsafe mode
- 5 sec. to run U-Boot console
- 7 sec. to run U-Boot netconsole
```
disini saya akan menahan tombol selama 5 second untuk mengintip dan berinteraksi dengan uboot console.

```
Reset button is pressed for:  5 

Button was pressed for 5 sec...
Starting U-Boot console...

uboot> help
?           - alias for 'help'
bootm       - boot application image from memory
cp          - memory copy
erase       - erase FLASH memory
help        - print embedded help
httpd       - start www server for firmware recovery
md          - memory display
mm          - memory modify (auto-incrementing)
mtest       - simple RAM test
mw          - memory write (fill)
nm          - memory modify (constant address)
ping        - send ICMP ECHO_REQUEST to network host
printenv    - print environment variables
printmac    - print MACtmodel  - print router model stored in flash
reset       - perform RESET of the CPU
setenv      - set environment variables
setmac      - save new MAC address in flash
startnc     - start net console
startsc     - start serial console
tftpboot    - boot image via network using TFTP protocol
version     - print U-Boot version
```
perintah yang paling suka saya gunakan untuk inspect memory biasanya adalah _md_ . Misalkan saya ingin melihat konten di 0x10000

```
uboot> md
Usage:
md [.b, .w, .l] address [# of objects]
        - memory display

uboot> md 10000
00010000: 04110002 00000000 80030000 03E0E021    ...............!
00010010: 8FE90000 0120E021 3C088000 35081000    ..... .!<...5...
00010020: 251D0000 8F990008 03200008 00000000    %........ ......
00010030: 0080E821 3C088001 8F8B000C 256B008C    ...!<.......%k..
00010040: 8D6AFFF4 00C04821 03807021 3C018001    .j....H!..p!<...
00010050: 0381E022 0386E020 038E7022 8D0B0000    ..."... ..p"....
00010060: AD2B0000 25080004 0148082A 1020FFFB    .+..%....H.*. ..
00010070: 25290004 20C8008C 01000008 00000000    %).. ...........
00010080: 8003065C 8003D310 00000113 8D0BFFFC    ...\............
00010090: 238C0008 240A0002 8D890000 11200002    #...$........ ..
000100A0: 012E4820 AD890000 214A0001 014B082A    ..H ....!J...K.*
000100B0: 1420FFF9 218C0004 8D09FFF4 8D0AFFF8    . ..!...........
000100C0: 012E4820 014E5020 2129FFFC 21290004    ..H .NP !)..!)..
000100D0: 012A082A 5420FFFD AD200000 00A02021    .*.*T ... .... !
000100E0: 8F990010 03200008 00C02821 3C098050    ..... ....(!<..P
000100F0: 2408FFFF AD280000 AD280004 AD280008    $....(...(...(..
```
untuk mengaktifkan fitur flashing via webui bisa dilanjutkan dengan perintah _httpd_.
```
uboot> httpd
Link down: eth1
Link down: eth0
Link down: eth1
Link down: eth0
Link down: eth1
Link down: eth0
Link down: eth1
Link down: eth0
Link down: eth1
Link down: eth0
Link down: eth1
Link down: eth0
Link down: eth1
Link down: eth0
Ethernet mode (duplex/speed): 1/1000 Mbps
HTTP server is starting at IP: 192.168.1.1
HTTP server is ready!
```
>Link down: eth* adalah notifikasi jika kabel LAN belum dicolok kesalah satu port. 

Disini kita diarahkan ke 192.168.1.1, buka lewat browser dan kita akan menemukan halaman upgrade via webui.

![alt text](/images/web_upgrade.png "webui")

Pilih firmware yang sesuai, upload dan proses flashing akan bekerja tanpa promting apapun. Tapi proses dibelakangnya tetap bisa kita initip.

* awalnya proses upload akan dilakukan ke addr _0x8080_ dengan besar upload _14155813 bytes_.

```
Request for: /
Request for: /style.css

Data will be downloaded at 0x8080
Upgrade type: fiwmware
Upload size: 14155813 bytes
         #######################################
         ##################################
         ###################################
         #####################################
         #####################################
         #################################
         #######################################
         #####################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         #######################################
         ###################################
         ###################################
         #####################################
         ###################################
         ##################################
         ####################################
         #######################################
         #######################################
         ####################################
         #######################################
         ###################################
         ##


HTTP upload is done! Upgrading...
```
* usai melakukan upload, maka dilanjutkan dengan mengahapus konten di _0x9F020000_ dengan besar _0xD80025_.

```
Executing: erase 0x9F020000 +0xD80025;
```


> _0xD80025_ adalah hexa dari _14155813_ 

* proses dilajutkan dengan copy konten di _0x80800000_ ke _0x9F020000_ dengan besar _0xD80025_.

```
cp.b 0x80800000 0x9F020000 0xD80025
```
terakhir baru kemudian di berikan dilakukan reboot dengan memory yang sudah di modifikasi sebelumnya.

```
bootm 0x9F020000
```

ringkasny seperti ini

```
HTTP upload is done! Upgrading...


****************************
*    FIRMWARE UPGRADING    *
* DO NOT POWER OFF DEVICE! *
****************************

Executing: erase 0x9F020000 +0xD80025; cp.b 0x80800000 0x9F020000 0xD80025
Erase flash from 0x9F020000 to 0x9FDAFFFF in bank #1
Erasing: ######################################
         #######################################
         #######################################
         #####################################
         #######################################
         ######################

Erased sectors: 217

Copying to flash...
Writting at address: 0x9F020000, , 14155813 bytes

*****************************************
*      U-Boot 1.1.4  (Jun 25 2014)      *
*****************************************

AP121 (AR9331) U-Boot for GL-iNet

DRAM:  64 MB
FLASH: Winbond W25Q128 (16 MB)
```

ref: [UBootCmdGroupFlash](https://www.denx.de/wiki/DULG/UBootCmdGroupFlash)
[1]:https://smrx86.github.io/boot-process_di_openwrt
[2]:https://github.com/pepe2k/u-boot_mod