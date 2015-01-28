---
layout: tips
title: Bina HTTP requests menggunakan nc
author: kamalmustafa
date: 2015-01-28
---

Untuk memeriksa *connection* dengan sesuatu laman web, saya biasanya menggunakan `telnet`
untuk melakukan *http requests* kepada *server* laman web tersebut. Tujuan utama adalah
untuk berhubung dengan server secara terus bagi memastikan masalah yang dihadapi tidak
disebabkan oleh aplikasi diperingkat lebih tinggi.

Contohnya untuk melakukan `GET` *request*:-

    telnet github.io 80
    Trying 199.27.75.133...
    Connected to github.map.fastly.net.
    Escape character is '^]'.
    GET /
    Connection closed by foreign host.
 
`telnet` mempunyai pelbagai masalah. Dalam contoh di atas, saya cuma dapat melakukan `GET`
*request* tanpa sempat menambah sebarang *HTTP headers* seperti `HOST` sebelum server tersebut
menutup *connection*.

Sesetengah *server* juga akan *time out* dengan cepat sekiranya mereka tidak menerima sebarang
data selepas *establish connection*. Lagi satu oleh kerana arahan di atas dimasukkan secara
interaktif, ianya tidak boleh diulang atau diautomasi dalam bentuk *script*.

Menggunakan `nc` nampaknya lebih baik. Contohnya untuk melakukan request beserta dengan `HOST`
*header* sekali:-

    echo -en "HEAD / HTTP/1.1\r\nHOST: k4ml.github.io\r\n\r\n" | nc k4ml.github.io 80

Dan anda akan mendapat output:-

    HTTP/1.1 200 OK
    Server: GitHub.com
    Content-Type: text/html
    Last-Modified: Fri, 12 Apr 2013 23:26:51 GMT
    Expires: Sun, 26 May 2013 19:40:06 GMT
    Cache-Control: max-age=600
    Content-Length: 9991
    Accept-Ranges: bytes
    Date: Sun, 26 May 2013 19:30:07 GMT
    Via: 1.1 varnish
    Age: 0
    Connection: keep-alive
    X-Served-By: cache-s34-SJC2
    X-Cache: MISS
    X-Cache-Hits: 0
    X-Timer: S1369596606.987305880,VS0,VE145
    Vary: Accept-Encoding
