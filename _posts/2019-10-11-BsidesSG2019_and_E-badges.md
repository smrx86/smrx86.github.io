---
layout: post
title: BsidesSG2019 & E-Badge Code
tags:
- Arduino
---

Selasa, 24 September 2019 saya berada di Singapura untuk menghadiri perehelatan penggiat keamanan IT yang dinamakan BsidesSG. Secara historis bsides mengacu pada konfrensi yang di inisasi oleh personal/komunitas yang mengalami penolakan tulisan untuk dipresentasikan di Blackhat USA 2009, penolakan ini terjadi karna terbatasnya waktu penyelenggaran BHUSA saat itu sedangkan antusias submit paper yang masuk sangat banyak. Bermula dari Las Vegas 2009, ide bsides lalu menyebar ke berbagai negara.

Dalam track utama ada kurang lebih 10 materi yang dipresentasikan, pembicara pertama **Vivek Ramachandran** cukup saya kenal serta materi tentang openwrt adalah bidang yang juga saya dalami. Didalam pembahasannya vivek membicarakan soal pemamfaatan metode LKM untuk menanamkan fungsi EDR kedalam router untuk melakukan logging semua koneksi yang masuk maupun keluar perangkat. 'Insmod' mungkin bukan cara favorit saya, cuma patut di coba mengingat tingkat kepraktisannya.

Pembahasan mengenai osquery juga sangat menarik untuk dibicarakan. Sederhananya sebuah deamon bisa mengubah semua output event OS kedalam bentuk SQL. Pencarian status program yang berjalan, listening port hingga ke status batterypun bisa di query.

![alt text](/images/osquery_battery.png "osquery")

Yang tak kalah menarik adalah celotehannya **Soya Aoyama** tentang bagaimana mengintip semua inputan maupun balikan event/string yang melewati library sebuah program dengan membuat library penyelia. Beberapa celutukan soal bagaimana effortnya melakukan submit bug dan kemudian microsoft menolak temuan metode local priveledge escalation untuk mengakali pembatasan UAC dikategorikan tidak qualified...ini benar-benar membuat riuh tawa seisi ruangan.

Materi lain yang sangat saya suka adalah pemaparan **Aliz Hammond** tentang bagaimana menggunakan command !pte !pfn di windbg untuk mengecek integriti sebuah file.

Semua materi bsidesSG 2019 sudah bisa di unduh lewat [tautan dihalaman resminya.](https://bsidessg.org/archives)

Electronic Badge
-
Desain [Abhinav Pandagale](https://twitter.com/TweetsFromPanda) untuk electonic badge sangat artistik mengambarkan ikon bajak laut (jack sparrow) dengan tambahan attiny85 tersolder dibawahnya. 

![alt text](/images/bsides_badges.jpg "pirates")

Di halaman [hackster.io](https://www.hackster.io/HacksFromPanda/pirates-of-the-singapore-bay-b9af0a) ia menuliskan sebuah code yang bisa mengemulasikan ketukan keyboard dan munuliskan sepotong puisi (seseorang yang mencari 8 bagian keramat di tanah para bajak laut).

Keyboard emulation/BADUSB pernah juga saya implementasion di [PIzero](https://www.youtube.com/watch?v=t7nV5bEDogM) sebelumnya. Untuk bisa berjalan di Mac OS code arduino attiny85 yang dituliskan Abhinav sepertinya membutuhkan sedikit penyesuaian seperti dibawah ini.

```
#include <DigiKeyboard.h>


void setup() {
    pinMode(1, OUTPUT); //onboard LED
}

void loop() {
  digitalWrite(1, HIGH); // setting onboard LED On
  DigiKeyboard.sendKeyStroke(0);
  DigiKeyboard.delay(1000); // 1 second delay for system to process
  DigiKeyboard.sendKeyStroke(KEY_SPACE, MOD_GUI_LEFT); //open spotlight
  DigiKeyboard.delay(500);
  DigiKeyboard.print("TextEdit"); // Starting texteditor
  DigiKeyboard.delay(500);
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("                                      Ahoy, Me Hearties!                                            ");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("                          Weigh Anchor and Hoist the Mizzen for BSides Singapore.                   ");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("                 Go run a rig, Hearties or heave on the Hackerwares");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("                       In search of coffer or Pieces of Eight");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("                                  In a distant land");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("                         You hoist your Jolly Roger high!");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE, MOD_GUI_LEFT); //open spotlight
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("terminal"); //Opening terminal
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.print("say $USER Eject the badge"); //speak from terminal
  DigiKeyboard.delay(10000); //Longer time delay
}
```
