---
layout: tips
title: Tentang DNS - A record vs CNAME
author: kamalmustafa
date: 2015-02-03
level: 2
summary: >
            Penerangan ringkas berkaitan konsep asas dns - A record vs CNAME
---

Ada beberapa jenis rekod dalam dns - A record, NS record, MX record, CNAME, TXT record etc. NS record cuma apply jika kita host dns server sendiri. Cuba fahamkan terlebih bagaimana dns berfungsi, secara ringkas:-

Kita daftar domain page.com dgn registrar - katakan Namecheap. Namecheap bagi kita control panel utk manage domain tersebut. Dalam control panel tersebut kita boleh nyatakan apakah dns server yang boleh resolv domain tersebut kepada IP address.

Katakan kita guna dns server daripada zenpipe dan zenpipe publish dns server mereka sebagai ns1.zenpipe.com. Jadi kita pun masukkan dalam control panel Namecheap ns1.zenpipe.com. Sekarang bila client cuba resolve domain page.com, dia tau dia perlu rujuk kepada ns1.zenpipe.com.

Zenpipe juga akan sediakan control panel utk kita manage dns record bagi page.com. Setelah login ke control panel zenpipe, kita boleh mula setup domain record utk page.com.

Mula2 sekali kita kena tentukan page.com nak point ke IP address mana ? Maka rekod dns kita buat permulaan mungkin seperti berikut.

    page.com IN A 100.22.22.22

Setiap domain perlu ada sekurang-kurangnya satu A record supaya domain tu point to something.

Setelah set A record, mungkin kita ada setup website baru - shop.page.com. Seperti yang dibincangkan sebelum ini, oleh kerana shop.page.com turut di host di server yang sama, maka kita cuma perlu setup CNAME kepada page.com.

    shop.page.com CNAME page.com

Ada buat website baru lagi, oleh kerana website ini agak besar, kita setup satu server baru dgn ip address 100.22.22.45. Maka kita tambah rekod dns seperti berikut:-

    bigsite.page.com IN A 100.22.22.45

Server yang digunakan utk host bigsite.page.com turut digunakan utk host support.page.com maka kita boleh setup CNAME spt berikut:-

    support.page.com CNAME bigsite.page.com

Setup satu website baru tapi guna service hosting kat test.test.com tapi test.com benarkan kita guna custom domain jadi boleh point custom domain tersebut kepada test.test.com:-

    blog.page.com CNAME test.test.com

Akhir sekali rekod dns kita akan kelihatan spt berikut:-

    page.com IN A 100.22.22.22
    shop.page.com CNAME page.com
    bigsite.page.com IN A 100.22.22.45
    support.page.com CNAME bigsite.page.com
    blog.page.com CNAME test.test.com

Nampak tak kaitan antara A record dan CNAME ?
