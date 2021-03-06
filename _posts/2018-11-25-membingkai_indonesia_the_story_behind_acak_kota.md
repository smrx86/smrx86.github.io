---
layout: post
title: Membingkai Indonesia, The Story Behind Acak Kota
---

>Supaya ada variasinya, maka tulisan iseng kali ini tidak akan bahas-bahas soal daleman wrt.

Bercerita sedikit tentang event BTP yang membawa saya dan team ke penjuru daerah di Indonesia. Beberapa diantaranya, Bandung, Malang, Palembang, Makassar (yg ini saya absen), Samarinda & Yogyakarta. Kami mengalami banyak hal keren selama tour tsb. Sebut saja camping di kantor, fitness gratis angkat beban + 30 kg, real bumpbump car ketika menuju bandara dll.

Soal sukses atau tidaknya terus terang saya sering ragu...kenapa? krn persiapan di venue kadang kurang dari sehari dari hari H. Pagi di hari H hampir selalu adalah bencana dan menguras tenaga. Tapi itu semua terbayarkan dengan antusias dari pesertanya. Meraka datang dari berbagai kalangan serta berbagai profesi, mulai dari pengemudi ojol, siswa, pengajar, mahasiswa hingga dosen. Bahkan ada peserta disabilitas ikut serta. 

Tak hanya itu, mereka kadang menempuh jarak puluhan kilometer dari dearah asal, naik bus malam dan begitu sampe sudah siang telat daftar ulang sehingga mendapatkan shift ujiannya sore menjelang magrib. Effort yang luar biasa dan patut diajungi 2 jempol.

Bagi saya sendiri, kadang berinteraksi dengan peserta ataupun team lokal membuat saya jadi sedikit tahu dengan budayanya, makanan khas dan yang pasti tujuan wisata. Disini saya sadar kalau __Indonesia Kaya.__

Terinspirasi dari "Indonesia nan kaya" dan untuk lebih mengenal pulahan kota di nusantara, maka di round 2 saya modifikasi satu soal ctf programing lama _jumble word_ menjadi acak kota. Ide awalnya soal ini adalah meyusun kata yang harus benar 50 kali berturut-turut kurang dalam waktu 60 detik. Dalam hal ini konten worlidst kata saya ganti dengan nama-nama kota yang mungkin terdengar asing bagi peserta termasuk saya sendiri.

Mustahil...ternyata tidak, ada banyak peserta yang solve soal ini. Salah satu pembahasan soal ini bisa di baca di [blog salah satu peserta](https://bayufedra.wordpress.com/2018/05/05/write-up-ctf-born-to-protect-kategori-programming/). 

Saya ucapkan selamat bagi anda2 yang sudah pernah mendapatkan flagnya dan bagi mereka yang mencoba manual saya harap ada nilai plus dengan anda mengenal nama kota di Indonesia.

Namun jika masih penasaran dengan soal nya berikut saya sertakan source codenya:

```
#!/usr/bin/env python

import socket
import time
from random import randrange
import random
import re
import os
import signal


flag = "btp{your_flag}"

def jumble(word):
	return ''.join(random.sample(word,len(word)))  	

def handle_client(conn,addr):
	WORDSAMPLESIZE 	= 25
	CORRECTANSWERS 	= 50
	ELAPSEDTIME 	= 60

	# seed after fork
	random.seed()

	#the banner that the players are greeted with
	BANNER = "\t\tSelamat datang di server Teka Teki Sulit.\n\t\t"
	BANNER += "="*40
	BANNER += "\n[+] susun %d nama kota ini kurang dari %d detik.\n" % (CORRECTANSWERS, ELAPSEDTIME)	

	#print the banner
	conn.send(BANNER)

	dictList = []
	ins = open( "namakota.txt", "r" )
	for line in ins:
		dictList.append(line.strip())
	ins.close()

	time.sleep(0.2)
	correctCounter=0
	start = int(time.time())

	for x in range(0,CORRECTANSWERS):
		
		#Send random word list range
		randomList = random.sample(dictList, WORDSAMPLESIZE)
		conn.send("Wordlist: %s\n" % repr(randomList))
		
		time.sleep(0.2)
		#Send random jumbled word
		serverWord = randomList[random.randint(0,len(randomList)-1)]
		conn.send("Nama kota yang di acak: %s\n" % jumble(serverWord))
		
		time.sleep(0.2)
		#Receive jumbled word
		conn.send("Masukkan jawabanmu: \n")
		clientWord = conn.recv(1024)
		
		if serverWord.strip() == clientWord.strip():
			correctCounter+=1
	
	elapsed = (int(time.time()) - start)
	print elapsed
	if correctCounter == CORRECTANSWERS and elapsed < ELAPSEDTIME:
		conn.send(flag)
	elif correctCounter == CORRECTANSWERS:
		conn.send("Kamu menyusunnya terlalu lama")
	else:
		conn.send("Kamu menjawab benar %d / %d soal" % (correctCounter,CORRECTANSWERS))
				
	conn.close()


print "Starting up simple programming challenge"
listensock = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
listensock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
listensock.bind(("0.0.0.0",7001))
print "Listening on port 7001"

#We dont care for zombies
signal.signal(signal.SIGCHLD,signal.SIG_IGN)

listensock.listen(1)
while True:
    (conn,address) = listensock.accept()
    print "Connection accepted from",address
    try:
        pid = os.fork()
    except:
        print "Error occurred when forking. Ignoring"
        continue
        
    if pid == 0:
        #Child Process - handle client then quit
        handle_client(conn,address)
        os._exit(0)
    else:
        #Parent process - Continue accepting connections
        print "Forking off child process PID=%d" % pid
        #Close the connected socket in the parent process
        conn.close()
        continue

listensock.close()
```


