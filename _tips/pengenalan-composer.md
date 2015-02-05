---
layout: tips
title: PHP: Pengenalan kepada Composer
author: kamalmustafa
date: 2015-02-05
level: 2
summary: >
            Penerangan ringkas tentang penggunaan Composer dalam aplikasi PHP.
---

Composer adalah tools untuk mendapatkan *dependencies* bagi projek PHP anda. Sebelum Composer, jika anda ingin menggunakan sebarang *library* luar, anda perlu download ia terlebih dahulu, unzip (jika dalam bentuk archive), dan kemudian *copy* ke folder projek anda. Bagaimana pula jika *library* tersebut turut bergantung kepada *library* lain ? Ulang semula proses sebelum ini.

```console
curl -sS https://getcomposer.org/installer | php
#!/usr/bin/env php
All settings correct for using Composer
Downloading...

Composer successfully installed to: /home/kamal/php/myshop/composer.phar
Use it: php composer.phar
```

