---
layout: post
title: 'Di Sebalik Form'
date: 2015-03-01
author: kamalmustafa
permalink: /di-sebalik-form.html
level: 2
summary: >
            Sebagai web developer, anda sudah biasa dengan proses membina form, dan kemudian
            menghantar (submit) form tersebut untuk di proses di server, sama ada menggunakan
            platform *server-side* seperti PHP, Python, .Net, Rails dan sebagainya.

            Tapi pernahkah anda terfikir, bagaimanakah data daripada *form* tersebut dihantar
            daripada *client* (*browser*) ke *server* ? Apakah format data tersebut ?
---
Sebagai web developer, anda sudah biasa dengan proses membina form, dan kemudian
menghantar (submit) form tersebut untuk di proses di server, sama ada menggunakan
platform *server-side* seperti PHP, Python, .Net, Rails dan sebagainya.

Tapi pernahkah anda terfikir, bagaimanakah data daripada *form* tersebut dihantar
daripada *client* (*browser*) ke *server* ? Apakah format data tersebut ?

<!--more-->

Asas kepada *web development* adalah [HTTP][http] (Hypertext Transfer Protocol). Ia adalah
protokol, atau satu bentuk piawai yang menetapkan bagaimanakah data daripada *client*
dihantar ke *server*. Yang bagusnya, protokol ini adalah berasaskan *text*, iaitu data
dihantar dalam bentuk *plain text*, berbanding sesetengah protokol lain yang memerlukan
data dihantar dalam bentuk *binaries encoding*.

Ini menjadikan HTTP tidak memerlukan sebarang peralatan (*tools*) khas jika kita ingin
membina client atau server yang boleh 'bercakap' dalam 'bahasa' HTTP. Apa saja yang boleh
menghantar dan menerima teks boleh digunakan.

<div class="admonition-warning">
    Protokol baru HTTP/2 bagaimanapun berbeza dengan HTTP/1.1 yang digunakan sekarang
    kerana ia tidak lagi berasaskan teks, tetapi dalam bentuk binari.
</div>
<div>&nbsp;</div>

Apakah contoh satu *tools* yang boleh menghantar teks dengan mudah ke server ? Telnet !

Ya, untuk menghantar dan menerima data melalui HTTP anda tidak perlu membina sebuah *browser*
seperti Firefox atau Google Chrome. Contoh, anda boleh meminta (*request*) resource daripada
laman web Google menggunakan telnet seperti berikut:-

```
telnet www.google.com 80
Trying 173.194.120.114...
Connected to www.google.com.
Escape character is '^]'.
GET /
HTTP/1.0 302 Found
Location: http://www.google.com.my/?gws_rd=cr&ei=xc3yVKKzMYuXuAS18oGABg
Cache-Control: private
Content-Type: text/html; charset=UTF-8
Set-Cookie: PREF=ID=48850ef362af9ca3:FF=0:TM=1425198533:LM=1425198533:S=6i5_XBkARR8adf6f; expires=Tue, 28-Feb-2017 08:28:53 GMT; path=/; domain=.google.com
Set-Cookie: NID=67=e2bqYqDM8NNvzFh0-2DoYBMXsff90Meli0u3otU0yxtBELBn7WbS7zqPOYE1gf8ukH7qb08OtGS8RLf96ECSlQuCcoTnGcYPRyUGY_8OA3dFnUOLKM1EFsMbVYovs5hT; expires=Mon, 31-Aug-2015 08:28:53 GMT; path=/; domain=.google.com; HttpOnly
P3P: CP="This is not a P3P policy! See http://www.google.com/support/accounts/bin/answer.py?hl=en&answer=151657 for more info."
Date: Sun, 01 Mar 2015 08:28:53 GMT
Server: gws
Content-Length: 262
X-XSS-Protection: 1; mode=block
X-Frame-Options: SAMEORIGIN
Alternate-Protocol: 80:quic,p=0.08

<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>302 Moved</TITLE></HEAD><BODY>
<H1>302 Moved</H1>
The document has moved
<A HREF="http://www.google.com.my/?gws_rd=cr&amp;ei=xc3yVKKzMYuXuAS18oGABg">here</A>.
</BODY></HTML>
Connection closed by foreign host.
```
Dalam contoh di atas, setelah berjaya *connect* ke server Google pada port 80, kita menghantar
*request* seperti berikut:-

```
GET /
```
*Request* di atas adalah sama dengan kita menaip di *address bar* browser `http://173.194.120.114/`.
Teks seterusnya adalah *response* yang kita dapat daripada *server* Google. 302 adalah HTTP status
daripada Google, manakala baris berikutnya merupakan *headers* yang menunjukkan url yang boleh kita
capai seterusnya. Inilah yang dinamakan *redirect* dan *HTTP client* seperti browser akan *redirect*
kita ke url yang diberikan.

Menggunakan telnet sebagai HTTP client bagaimanapun agak terhad apa yang kita boleh lakukan. Dalam
contoh di atas, selepas *connect* ke Google, saya hanya boleh taip sebaris `GET /` dan Google terus
menghantar response. Saya tidak sempat untuk menghantar *headers* lain seperti `HOST` dan sebagainya. 
Satu lagi *tools* lain yang lebih fleksibel adalah `nc`. Saya ada [menulis][nc] mengenai penggunaanya sebelum
ini.

Berbalik kepada tajuk tulisan ini, mari kita lihat apakah data yang dihantar apabila kita submit *form*
pendaftaran di laman Facebook.com. Untuk melihat data yang dihantar, saya menggunakan kaedah yang paling
*simple*, iaitu menukar bahagian `<form action=...>` *form* tersebut ke tempat lain.

<img src="http://i.imgur.com/QUF21Lj.png"></img>
<img src="http://i.imgur.com/rccFhbx.png"></img>

Dalam *screenshot* di atas, saya telah menukar `<form action=...>` *form* tersebut ke:-

```
http://localhost:4000/
```
Ini adalah kaedah paling *simple*. Cara lain untuk 'menangkap' data yang dihantar adalah dengan menggunakan
perisian seperti Wireshark atau `tcpdump`. Saya lebih gemarkan cara ini kerana data yang kita dapat lebih
*raw* dan tidak melalui sebarang bentuk *processing*. Ia betul-betul data yang diterima daripada browser
pada *socket* di *port* 4000. Untuk menerima data tersebut, kita hanya perlu menggunakan perisian bernama
netcat atau nc. Jalankan arahan berikut:-

```
nc -l 4000
```
Arahan diatas meminta `nc` *listen* pada *port* 4000, dan bersedia menerima sebarang data yang dihantar
melalui *port* tersebut. Setelah kita *submit* *form* pendaftaran Facebook tadi, kita akan dapati teks
dibawah dipaparkan pada konsol dimana `nc` tadi dijalankan.

Ya, inilah contoh data yang dihantar, apabila kita *submit* form pada satu-satu laman web, dalam contoh
ini laman Facebook.com.

```
POST / HTTP/1.1
Host: localhost:4000
Connection: keep-alive
Content-Length: 1022
Cache-Control: max-age=0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Origin: https://www.facebook.com
User-Agent: Mozilla/5.0 (X11; Linux i686) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/38.0.2125.111 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip,deflate
Accept-Language: en-US,en;q=0.8

lsd=AVpzsLKs&firstname=Kamal&lastname=Mustafa&reg_email__=kmkml%40yahoo.com&reg_email_confirmation__=kmkml%40yahoo.com&reg_passwd__=abc123&birthday_month=7&birthday_day=26&birthday_year=1991&sex=2&websubmit=&referrer=&asked_to_login=&terms=on&ab_test_data=&reg_instance=l_byVOe0tUkXB8lG29Dte0Zj&contactpoint_label=email_or_phone&locale=en_US&captcha_persist_data=AZlcaJSuaa-rSMdsK8CsS8tRNMgau9HFF2b33fK7a-ydHKi7emqwu3y-_E8fOuL3m3e5SDmXANKzSRspsF1O6ugJdZ_aCGJy0QHpi4JU0JpDypQIx2_WpbzT6OV1UhSetMP05to2mFtVK0hS50NWSbZ1BJYv_ygKP5tt1BRCEiUHyW9-hxLG-nrWUF7DBlyMeEiyDLdknXabXEtfdqMpToPOxvsb1qMymwv-9fibK9CHGGV1yD8bK3RlhSWVdsDk2Rl1V8d0-PkynVcXbT5GaJBDy-bBtctrMHHHTjzIIGATS5mKr7Dyu4qrDuUEYC3L7ipfExwoewlqrISKoTyoUhwsWhageWVrm5f7IcnRHyLeIg&captcha_session=CnTabJ0C4tNIiH63kxKnrw&extra_challenge_params=authp%3Dnonce.tt.time.new_audio_default%26psig%3DjAKLb4KI5WqB-zg_RE5QcLV8sss%26nonce%3DCnTabJ0C4tNIiH63kxKnrw%26tt%3DRXdPBAAUdMa1cxL9bQ0irk2F0lQ%26time%3D1425209003%26new_audio_default%3D1&recaptcha_type=password&captcha_response=
```

Di bahagian [*server-side*](/client-side-vs-server-side.html), data di atas kemudian akan diproses ke dalam bentuk yang lebih
mudah untuk *developer* gunakan. Contohnya dalam platform PHP, anda akan mendapat [data di atas dalam bentuk `$_GET` atau `$_POST`](http://k4ml.github.io/ms/posts/bagaimana-_post-dan-_get-terhasil.html).

Kita boleh meneruskan eksperimen ini untuk melihat situasi-situasi lain, contohnya apabila *submit* form menggunakan
jQuery.AJAX contohnya tetapi saya akan berhenti disini terlebih dahulu.

Poin terpenting yang saya ingin rumuskan disini
adalah apabila kita memahami bagaimana sesuatu teknologi itu berfungsi pada tahap yang paling asas, ia akan lebih membantu
kita dalam menyelesaikan permasalahan berkaitan teknologi tersebut.

Sentiasa ajukan persoalan kepada diri sendiri, bagaimana sesuatu benda itu berfungsi dan jalankan eksperimen untuk
mendapatkan jawapannya.

[nc]:http://metak4ml.blogspot.com/2013/05/craft-http-requests-using-nc.html
[http]:https://www.ietf.org/rfc/rfc2616.txt
