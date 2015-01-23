---
layout: post
title: Cara Murah Guna SSL (HTTPS) Untuk Laman Web Anda
author: kamalmustafa
date: 2015-01-20
level: 2
summary: >
            Penggunaan SSL (HTTPS) adalah penting tapi memerlukan penggunaan dedicated IP
            bagi setiap website. Bagaimana SNI membantu menyelesaikan masalah ini ?
---

Penggunaan SSL (HTTPS) adalah penting jika laman web anda melibatkan data-data pengguna
seperti username, password, maklumat pembelian, data peribadi seperti nombor kad pengenalan
dan sebagainya. Data-data penting ini begitu mudah untuk dicuri jika pengguna menggunakan
capaian Internet di tempat awam.

<!--more-->

Untuk menggunakan HTTPS bagaimana pun memerlukan setiap website mempunyai alamat IP yang
tersendiri. Kebanyakkan laman web pula menggunakan perkhidmatan *shared hosting* sebagai
hos dan ini bermakna mereka berkongsi alamat IP dengan pelbagai laman lain di hos yang sama.

Jadi untuk menggunakan HTTPS, mereka perlu menyewa alamat IP tambahan, khusus untuk laman web
mereka sahaja.

*Server Name Indication* (SNI) adalah teknologi yang membolehkan penggunaan *SSL certificate*
pada website tanpa memerlukan *dedicated* IP. Kebanyakkan browser hari ini menyokong SNI.
Untuk peranti mobile android, support bermula dengan Android 3.0 (HoneyComb) manakala untuk
IoS (Iphone) bermula dgn versi 4.0.

Ini bermakna peranti yang dibeli dalam tempoh 4 tahun lepas sudah pun mempunyai sokongan terhadap SNI.
SNI (Server Name Indication) boleh memangkin penggunaan HTTPS yang lebih meluas.

Jika anda menggunakan *shared hosting* dan ingin menggunakan HTTPS bagi laman web anda, hubungi penyedia
*shared hosting* anda untuk bertanyakan sama ada mereka sudah mempunyai sokongan untuk SNI atau
tidak.
