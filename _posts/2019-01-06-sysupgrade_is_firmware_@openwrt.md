---
layout: post
title: Sysupgrade is Firmware Upgrade @Openwrt
tags:
- openwrt
---

Sesuai title, ini adalah ulasan singkat soal upgrade firmware. Tapi sebelumnya saya akan rangkum dalam bentuk gambar soal dimana posisi sysupgrade dalam konteks bootproses di openwrt.

![alt text](/images/boot1.png "bootprocess")

> Pendek kata sysupgrade adalah opsi pembaharuan firmware dalam kondisi booting proses berjalan normal. 

Di web UI biasanya kita temukan backup/flash firmaware sebagai versi GUI dari sysupgrade. Jadi ketika anda memilih upload firmware -> flash firmware sebenarnya proses yang berjalan dibelakang adalah sysupgrade.

Upload firmware akan mengcopy *.bin ke dalam folder /tmp/firmware.bin.

process selanjutnya dihandle oleh sysupgrade untuk melakukan fresh upgrade dengan opsi _-n_,

```
root@Openwrt:/# sysupgrade -vn /tmp/*.bin  

killall: watchdog: no process killed
Sending TERM to remaining processes ... ubusd askfirst logd logread netifd odhcpd snmpd uhttpd ntpd dnsmasq
Sending KILL to remaining processes ... askfirst
Switching to ramdisk...
Performing system upgrade...
Unlocking firmware ...
Writing from <stdin> to firmware ...  [w]
Appending jffs2 data from /tmp/sysupgrade.tgz to firmware...TRX header not found
Error fixing up TRX header
Upgrade completed
Rebooting system...
```
ref: [Sysupgrade – Technical Reference](https://openwrt.org/docs/techref/sysupgrade)