---
layout: post
title: Boot Process @Openwrt
---

Beberapa tahun lalu saya berkeinginan untuk mengulas tentang proses booting di openwrt, tapi sering terkendala. Berhubung kemarin saya bikin kabel serial usb uart, kepikiran...kenapa tidak tulis sekalian biar bisa jadi dokumentasinya. Perangkat yang saya gunakan adalah GLinet 6416 yang terhubung lewat kabel usb2uart buatan dari kabel nokia ca 42.
![alt text](https://scontent-sin2-1.cdninstagram.com/vp/65c76b33a22579daecbd90e2d87a6606/5CB1473A/t51.2885-15/e35/44760113_267062790821960_1238197665365512068_n.jpg "ca42")

Ada sedikit perbedaan antara booting proses di OS linux desktop dengan *nux embedded. Jika di linux kita mengenal ada 6 level proses (Bios, MBR, GRUB, Kernel, Init, Runlevel), maka di openwrt cukup hanya 3 tahapan:

Bootloader
-
 * di proses ini bootloader (ex:u-boot) di muat dari flash memory, 
 * setelah bootloader berjalan maka dilajutkan dengan initialized hardware.
 * setelah proses initialized maka barulah kernel image dimuat dari flash memory ke RAM memory dalam partisi mtd.

 
```
*****************************************
*      U-Boot 1.1.4  (Jun 25 2014)      *
*****************************************

AP121 (AR9331) U-Boot for GL-iNet

DRAM:  64 MB
FLASH: Winbond W25Q128 (16 MB)

LED on during eth initialization...

Hit any key to stop autobooting:  0 

Booting image at: 0x9F020000

   Image name:   OpenWrt 2.1.0-RC2-52
   Image type:   MIPS Linux Kernel Image (lzma compressed)
   Data size:    1157976 Bytes = 1.1 MB
   Load address: 0x80060000
   Entry point:  0x80060000

Uncompressing kernel image... OK!
Starting kernel...

[    0.000000] Linux version 3.18.84 (@01ce795a5a29) (gcc version 4.8.3 (OpenWrt/Linaro GCC 4.8-2014.04 unknown) ) #238 8
[    0.000000] bootconsole [early0] enabled
[    0.000000] CPU0 revision is: 00019374 (MIPS 24Kc)
[    0.000000] SoC: Atheros AR9330 rev 1
[    0.000000] Determined physical RAM map:
[    0.000000]  memory: 04000000 @ 00000000 (usable)
[    0.000000] Initrd not found or empty - disabling initrd
[    0.000000] Zone ranges:
[    0.000000]   Normal   [mem 0x00000000-0x03ffffff]
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x00000000-0x03ffffff]
[    0.000000] Initmem setup node 0 [mem 0x00000000-0x03ffffff]
[    0.000000] Primary instruction cache 64kB, VIPT, 4-way, linesize 32 bytes.
[    0.000000] Primary data cache 32kB, 4-way, VIPT, cache aliases, linesize 32 bytes
[    0.000000] Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 16256
[    0.000000] Kernel command line:  board=PINEAPPLE-NANO  console=ttyATH0,115200 rootfstype=squashfs,jffs2 noinitrd
[    0.000000] PID hash table entries: 256 (order: -2, 1024 bytes)
[    0.000000] Dentry cache hash table entries: 8192 (order: 3, 32768 bytes)
[    0.000000] Inode-cache hash table entries: 4096 (order: 2, 16384 bytes)
[    0.000000] Writing ErrCtl register=00000000
[    0.000000] Readback ErrCtl register=00000000
[    0.000000] Memory: 60880K/65536K available (2524K kernel code, 143K rwdata, 540K rodata, 240K init, 188K bss, 4656K )
[    0.000000] SLUB: HWalign=32, Order=0-3, MinObjects=0, CPUs=1, Nodes=1
[    0.000000] NR_IRQS:51
[    0.000000] Clocks: CPU:400.000MHz, DDR:400.000MHz, AHB:200.000MHz, Ref:25.000MHz
[    0.000000] Calibrating delay loop... 265.42 BogoMIPS (lpj=1327104)
[    0.080000] pid_max: default: 32768 minimum: 301
[    0.080000] Mount-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.090000] Mountpoint-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.100000] NET: Registered protocol family 16
[    0.100000] MIPS: machine is WiFi Pineapple NANO
[    0.370000] Switched to clocksource MIPS
[    0.370000] NET: Registered protocol family 2
[    0.380000] TCP established hash table entries: 1024 (order: 0, 4096 bytes)
[    0.380000] TCP bind hash table entries: 1024 (order: 0, 4096 bytes)
[    0.380000] TCP: Hash tables configured (established 1024 bind 1024)
[    0.390000] TCP: reno registered
[    0.390000] UDP hash table entries: 256 (order: 0, 4096 bytes)
[    0.400000] UDP-Lite hash table entries: 256 (order: 0, 4096 bytes)
[    0.410000] NET: Registered protocol family 1
[    0.420000] futex hash table entries: 256 (order: -1, 3072 bytes)
[    0.440000] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    0.440000] jffs2: version 2.2 (NAND) (SUMMARY) (LZMA) (RTIME) (CMODE_PRIORITY) (c) 2001-2006 Red Hat, Inc.
[    0.450000] msgmni has been set to 118
[    0.460000] io scheduler noop registered
[    0.460000] io scheduler deadline registered (default)
[    0.460000] Serial: 8250/16550 driver, 1 ports, IRQ sharing disabled
[    0.470000] ar933x-uart: ttyATH0 at MMIO 0x18020000 (irq = 11, base_baud = 1562500) is a AR933X UART
[    0.480000] console [ttyATH0] enabled
[    0.480000] console [ttyATH0] enabled
[    0.490000] bootconsole [early0] disabled
[    0.490000] bootconsole [early0] disabled
[    0.500000] m25p80 spi0.0: found w25q128, expected m25p80
[    0.500000] m25p80 spi0.0: w25q128 (16384 Kbytes)
[    0.510000] 5 tp-link partitions found on MTD device spi0.0
[    0.510000] Creating 5 MTD partitions on "spi0.0":
[    0.520000] 0x000000000000-0x000000020000 : "u-boot"
[    0.520000] 0x000000020000-0x00000013ad58 : "kernel"
[    0.530000] 0x00000013ad58-0x000000ff0000 : "rootfs"
[    0.530000] mtd: device 2 (rootfs) set to be root filesystem
[    0.540000] 1 squashfs-split partitions found on MTD device rootfs
[    0.540000] 0x000000d40000-0x000000ff0000 : "rootfs_data"
[    0.550000] 0x000000ff0000-0x000001000000 : "art"
[    0.550000] 0x000000020000-0x000000ff0000 : "firmware"
[    0.580000] libphy: ag71xx_mdio: probed
[    1.170000] ag71xx ag71xx.0: connected to PHY at ag71xx-mdio.1:04 [uid=004dd041, driver=Generic PHY]
[    1.180000] eth0: Atheros AG71xx at 0xb9000000, irq 4, mode:MII
[    1.180000] TCP: cubic registered
[    1.180000] NET: Registered protocol family 17
[    1.190000] bridge: automatic filtering via arp/ip/ip6tables has been deprecated. Update your scripts to load br_netf.
[    1.200000] 8021q: 802.1Q VLAN Support v1.8
[    1.210000] VFS: Mounted root (squashfs filesystem) readonly on device 31:2.
[    1.220000] Freeing unused kernel memory: 240K
[    4.670000] init: Console is alive
[    4.670000] init: - watchdog -
[    8.000000] usbcore: registered new interface driver usbfs
[    8.000000] usbcore: registered new interface driver hub
[    8.010000] usbcore: registered new device driver usb
[    8.060000] SCSI subsystem initialized
[    8.070000] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[    8.080000] ehci-platform: EHCI generic platform driver
[    8.080000] ehci-platform ehci-platform: EHCI Host Controller
[    8.090000] ehci-platform ehci-platform: new USB bus registered, assigned bus number 1
[    8.100000] ehci-platform ehci-platform: irq 3, io mem 0x1b000000
[    8.120000] ehci-platform ehci-platform: USB 2.0 started, EHCI 1.00
[    8.120000] hub 1-0:1.0: USB hub found
[    8.120000] hub 1-0:1.0: 1 port detected
[    8.130000] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
[    8.140000] ohci-platform: OHCI generic platform driver
[    8.140000] uhci_hcd: USB Universal Host Controller Interface driver
[    8.160000] usbcore: registered new interface driver usb-storage
```

>lihat ada log _Hit any key to stop autobooting:  0_   diawal proses bootloader, itu adalah penanda proses sela yang digunakan untuk melakukan injeksi ke flash memori jika perngkat mengalami brick (akan saya bahas lain kali).

Kernel
-
Disini semua processs yang tercantum di dalam _/etc/preinit_ akan di jalankan.

```
---- content of /etc/preinit ----
[ -z "$PREINIT" ] && exec /sbin/init

export PATH=/usr/sbin:/usr/bin:/sbin:/bin

pi_ifname=
pi_ip=192.168.1.1
pi_broadcast=192.168.1.255
pi_netmask=255.255.255.0

fs_failsafe_ifname=
fs_failsafe_ip=192.168.1.1
fs_failsafe_broadcast=192.168.1.255
fs_failsafe_netmask=255.255.255.0

fs_failsafe_wait_timeout=2

pi_suppress_stderr="y"
pi_init_suppress_stderr="y"
pi_init_path="/usr/sbin:/usr/bin:/sbin:/bin"
pi_init_cmd="/sbin/init"

. /lib/functions.sh
. /lib/functions/preinit.sh
. /lib/functions/system.sh

boot_hook_init preinit_essential
boot_hook_init preinit_main
boot_hook_init failsafe
boot_hook_init initramfs
boot_hook_init preinit_mount_root

for pi_source_file in /lib/preinit/*; do
	. $pi_source_file
done

boot_run_hook preinit_essential

pi_mount_skip_next=false
pi_jffs2_mount_success=false
pi_failsafe_net_message=false

boot_run_hook preinit_main
```

 * ketika load akan terlihat

```
[    8.970000] init: - preinit -
[   11.350000] random: procd urandom read with 17 bits of entropy available
[   11.770000] mount_root: loading kmods from internal overlay
[   12.340000] jffs2: notice: (324) jffs2_build_xattr_subsystem: complete building xattr subsystem, 2 of xdatum (0 unche.
[   12.350000] block: attempting to load /tmp/jffs_cfg/upper/etc/config/fstab
[   12.370000] block: extroot: not configured
[   12.410000] jffs2: notice: (321) jffs2_build_xattr_subsystem: complete building xattr subsystem, 2 of xdatum (0 unche.
[   12.530000] block: attempting to load /tmp/jffs_cfg/upper/etc/config/fstab
[   12.550000] block: extroot: not configured
[   12.550000] mount_root: switching to jffs2 overlay
```

Init
-
* disini konten _/etc/inittab_ di muat

```
[   12.610000] procd: - early -
[   12.610000] procd: - watchdog -
[   13.770000] procd: - ubus -
[   14.960000] procd: - init -
Please press Enter to activate this console.
[   21.520000] Loading modules backported from Linux version v4.4-rc5-1913-gc8fdf68
[   21.520000] Backport generated by backports.git backports-20151218-0-g2f58d9d
[   21.680000] ieee80211 phy0: Atheros AR9330 Rev:1 mem=0xb8100000, irq=2
[   21.690000] usbcore: registered new interface driver ath9k_htc
[   21.720000] RPC: Registered named UNIX socket transport module.
[   21.730000] RPC: Registered udp transport module.
[   21.730000] RPC: Registered tcp transport module.
[   21.740000] RPC: Registered tcp NFSv4.1 backchannel transport module.
[   21.780000] tun: Universal TUN/TAP device driver, 1.6
[   21.780000] tun: (C) 1999-2004 Max Krasnyansky <maxk@qualcomm.com>
[   21.830000] usbcore: registered new interface driver rt2800usb
[   21.840000] usbcore: registered new interface driver rtl8187
[   21.880000] usbcore: registered new interface driver rtl8192cu
[   21.970000] usbcore: registered new interface driver cdc_acm
[   21.980000] cdc_acm: USB Abstract Control Model driver for USB modems and ISDN adapters
[   22.000000] usbcore: registered new interface driver cdc_wdm
[   22.020000] nf_conntrack version 0.5.0 (955 buckets, 3820 max)
[   22.060000] usbcore: registered new interface driver ums-alauda
[   22.070000] usbcore: registered new interface driver ums-cypress
[   22.070000] usbcore: registered new interface driver ums-datafab
[   22.080000] usbcore: registered new interface driver ums-freecom
[   22.090000] usbcore: registered new interface driver ums-isd200
[   22.090000] usbcore: registered new interface driver ums-jumpshot
[   22.100000] usbcore: registered new interface driver ums-karma
[   22.110000] usbcore: registered new interface driver ums-sddr09
[   22.120000] usbcore: registered new interface driver ums-sddr55
[   22.130000] usbcore: registered new interface driver ums-usbat
[   22.150000] usbcore: registered new interface driver usbserial
[   22.150000] usbcore: registered new interface driver usbserial_generic
[   22.160000] usbserial: USB Serial support registered for generic
[   22.210000] xt_time: kernel timezone is -0000
[   22.220000] usbcore: registered new interface driver asix
[   22.230000] usbcore: registered new interface driver ax88179_178a
[   22.240000] usbcore: registered new interface driver cdc_ether
[   22.250000] ip_tables: (C) 2000-2006 Netfilter Core Team
[   22.270000] usbcore: registered new interface driver pl2303
[   22.270000] usbserial: USB Serial support registered for pl2303
[   22.290000] PPP generic driver version 2.4.2
[   22.290000] NET: Registered protocol family 24
[   22.300000] usbcore: registered new interface driver qmi_wwan
[   22.310000] usbcore: registered new interface driver rndis_host
[   22.320000] usbcore: registered new interface driver sierra_net
[   22.330000] usbcore: registered new interface driver option
[   22.340000] usbserial: USB Serial support registered for GSM modem (1-port)
[   36.230000] device eth0 entered promiscuous mode

Pineapple login: [   43.390000] device wlan0 entered promiscuous mode
[   43.480000] br-lan: port 2(wlan0) entered forwarding state
[   43.480000] br-lan: port 2(wlan0) entered forwarding state
[   45.480000] br-lan: port 2(wlan0) entered forwarding state
[   63.850000] random: nonblocking pool is initialized
```
> di proses inilah fitur-fitur keamanan yang dicostumize biasanya ditempatkan, misal dalam _/etc/inittab_ ada _::askconsole:/bin/login_ untuk memproteksi serial console dengan mengharuskan login terlebih dahulu.

Untuk hari cukup ini dulu ya. c'ya.  

 


