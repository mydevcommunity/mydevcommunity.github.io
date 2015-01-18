---
layout: post
title: GitHub Sebagai Hos Laman Web
author: ikhwanhayat
---

[GitHub](https://github.com) adalah satu servis di mana anda boleh meletakkan kod sumber perisian anda dan boleh menjemput pengaturcara lain untuk berkolaborasi membangunkan perisian tersebut. Namun disamping itu, GitHub juga menyediakan servis di mana anda boleh meletakkan laman web anda di situ. Perkhidmatan ini dinamakan [GitHub Pages](https://pages.github.com) dan disediakan secara percuma!

<!--more-->

Artikel ini akan mengandaikan anda sudah biasa dengan GitHub dan [Git](http://git-scm.com), kerana jika ingin menerangkan tentang kedua-duanya maka akan jadi panjang ceritanya nanti (mungkin lain kali :).

Untuk menggunakan GitHub Pages, anda mestilah membuat satu repositori yang dinamakan "_usernameanda_.github.io". Seterusnya anda perlu masukkan halaman-halaman web anda yang di dalam bentuk fail HTML, imej, dan sebagainya ke dalam repositori tersebut. Kemudian, anda boleh terus melayarinya di "http://usernameanda.github.io". Semudah itu!

Panduan langkah demi langkah memulakan GitHub Pages boleh anda dapati di [halaman utama GitHub Pages](https://pages.github.com/). Sebenarnya ada dua cara penggunaan GitHub Pages, iaitu sama ada laman web untuk _user/organization_ atau untuk _project_. Dokumentasi penuh anda boleh baca di [laman bantuan GitHub Pages](https://help.github.com/categories/github-pages-basics/).

Untuk lebih jelas, mari kita ambil satu contoh. Laman web MyDev ini sendiri sebenarnya dihoskan oleh GitHub Pages. Username yang kami gunakan ialah "mydevcommunity" dan profil GitHub kami boleh dilihat di [https://github.com/mydevcommunity](https://github.com/mydevcommunity). Kandungan laman web ini kami letakkan di repositori [mydevcommunity.github.io](https://github.com/mydevcommunity/mydevcommunity.github.io). Ini membolehkan ia dilayari di [http://mydevcommunity.github.io](http://mydevcommunity.github.io). 

Domain "www.mydev.my" sebenarnya dihalakan ke domain "mydevcommunity.github.io". Cara untuk menggunakan domain anda sendiri bersama GitHub Pages ada di [sini](https://help.github.com/articles/about-custom-domains-for-github-pages-sites/).

Jika anda perhatikan dalam repositori tersebut, kami sebenarnya tidak hoskan terus fail-fail HTML, tetapi kami menggunakan sejenis CMS (_Content Management System_) dinamakan [Jekyll](http://jekyllrb.com).

GitHub Pages menyokong penggunaan Jekyll di mana pengguna tidak perlu menguruskan fail-fail HTML satu per satu. Kandungan boleh ditulis dalam format [Markdown ](http://daringfireball.net/projects/markdown/) dan kemudian Jekyll akan menjana fail-fail HTML secara automatik berdasarkan _layout_ dan konfigurasi yang kita telah tetapkan sebelum itu.

Maklumat lanjut integrasi GitHub Pages dan Jekyll boleh dibaca di [sini](https://help.github.com/articles/using-jekyll-with-pages/)

Antara kelebihan penggunaan GitHub Pages ini adalah:

1. Ia percuma :)
1. Anda boleh menggunakan _social feature_ dalam GitHub untuk berkolaborasi membangunkan laman web anda, seperti melalui penggunaan _pull request_.
1. Hanya gunakan Git untuk _publish_ laman web anda, tak perlu FTP atau sebagainya.
1. Secara tidak langsung, segala perubahan pada kod sumber laman web anda akan direkodkan.

Jadi, melalui GitHub Pages, anda boleh membina laman web korporat anda, laman peribadi, laman komuniti, atau pun blog dengan mudah dan murah.