---
layout: post
title: Fun With RaMBLE
tags:
- BLE hacking
---

Bicara tentang Bluetooth low energy, ini adalah [topik lain](https://www.slideshare.net/Ramaporter/hackingblesmartwatch-idsecconf2019-cirebon) yang sudah saya angkat di acara Idsecconf 2019 di Cirebon bulan maret yang lalu. Bahan-bahan papernya sendiri sebenarnya sudah lebih setahun tepat sesudah pembelian amazfit bip.

Beberapa minggu usai Idsecconf seorang teman membagikan info soal adanya applikasi yang bisa digunakan untuk BLE wardriving.
  
![alt text](/images/ramblegoogleplay.png "googlepay")

Applikasi yang cukup menarik dan baru minggu lalu saya sempatkan untuk meexplore sedikit fitur yang dimiliki [RaMBLE](https://play.google.com/store/apps/details?id=com.contextis.android.BLEScanner&hl=in). Bagian yang paling menonjol adalah kemampuan mapping, info advertise BLE serta timepstampnya.

![alt text](/images/ramblemap.jpg "ramblemap")

Dilihat dari konten database yang dikumpulkan sebenarnya ada banyak info yang bisa ditampilkan. Tapi tentu tidak begitu penting juga jika tampil di setiap node di peta. Isi database ramble hampir bisa menyamai hasil scanning (bukan enumerate) yang biasa saya dapatkan dengan tool bleah.

![alt text](/images/rambledatabase.png "rambledatabase")

![alt text](/images/bleahscabip.png "bleahscabip")

Namun sayangnya database yang dimiliki applikasi ini tidak dapat diexport langsung kedalam bentuk kml. Untuk itu hasil databasenya yang berbentuk sqlite perlu sedikit diolah dan disimpan kedalam bentuk yang memungkinkan diterima oleh google maps.

Berbekal sedikit pengetahuan tentang query command di sqlite kita bisa menggabungkan baris isi dari tabel devices dan locations.

```
$sqlite3
sqlite> .open RaMBLE_playstore_*.*_*.sqlite
sqlite> .headers on
sqlite> .mode csv
sqlite> .output data.csv
sqlite> select devices.device_name, devices.device_type, devices.address, locations.latitude, locations.longitude as id from devices, locations where devices.id = locations.device_id;
sqlite> .quit 
```
Dari hasil ouput csv file kita menggunakan [python script](https://github.com/flynnpc/CSV-to-KML) atau tool online untuk mengubah ke format KML dan siap di upload ke google maps.

![alt text](/images/rumble2gmaps.png "rumble2gmaps")

1,5 jam menggunkan applikasi ini dari naik KRL hingga jalan kaki dari stasiun saya dapatkan lebih dari cukup (532 ble mac address yang muncul dalam 2089 lokasi).

> Percaya atau tidak riset soal ble masih sangat seksi dan menyenangkan, lihat saja demo presentasi [Damien Cauquil](https://media.defcon.org/DEF%20CON%2027/DEF%20CON%2027%20presentations/DEFCON-27-Damien-Cauquil-Defeating-Bluetooth-Low-Energy-5-PRNG-for-fun-and-jamming-Demo-Video/) di defcon 27 atau tulisan Jokull Barkson di ezine [PageOut_001](https://pagedout.institute/download/PagedOut_001_beta1.pdf).

