---
layout: tips
title: 'PHP: Pengenalan kepada Composer'
author: kamalmustafa
date: 2015-02-05
level: 2
summary: >
            Penerangan ringkas tentang penggunaan Composer dalam aplikasi PHP.
---

Composer adalah tools untuk mendapatkan *dependencies* bagi projek PHP anda. Sebelum Composer, jika anda ingin menggunakan sebarang *library* luar, anda perlu download ia terlebih dahulu, unzip (jika dalam bentuk archive), dan kemudian *copy* ke folder projek anda. Bagaimana pula jika *library* tersebut turut bergantung kepada *library* lain ? Ulang semula proses sebelum ini.

Terlebih dahulu anda perlu install `composer`. Ia boleh dilakukan melalui *command* berikut:-

```console
curl -sS https://getcomposer.org/installer | php
```
Anda akan mendapat *output* seperti berikut:-

```console
#!/usr/bin/env php
All settings correct for using Composer
Downloading...
Composer successfully installed to: /home/kamal/php/myshop/composer.phar
Use it: php composer.phar
```
Seterusnya, untuk men'download' *library* yang anda inginkan, bina fail bernama `composer.json` seperti berikut:-

```json
{
    "require": {
        "swiftmailer/swiftmailer": "5.3.1"
    }
}
```

Kemudian, jalankan arahan berikut:-

```console
php composer.phar install
```
Anda akan mendapat output lebih kurang seperti berikut apabila selesai:-

<a href="http://imgur.com/tVjCE09"><img src="http://i.imgur.com/tVjCE09.png" title="source: imgur.com" /></a>

<div class="admonition-info">
    Perhatikan satu folder baru bernama <code>vendor</code> dibina. Ia akan mengandungi kesemua code yang di'download' melalui <code>composer</code>.
</div>
<div>&nbsp;</div>

Setelah `composer` selesai download, anda boleh mula menggunakan library tersebut dalam *script* PHP anda dengan hanya menambah baris berikut:-

```php
<?php
require 'vendor/autoload.php';
```

Contohnya, bina fail baru bernama `mail.php` seperti berikut:-

```php
<?php
require 'vendor/autoload.php';

$transport = Swift_SmtpTransport::newInstance('127.0.0.1', 25);
$mailer = Swift_Mailer::newInstance($transport);

$message = Swift_Message::newInstance('Wonderful Subject')
  ->setFrom(array('john@doe.com' => 'John Doe'))
  ->setTo(array('kamal-test@mailinator.com'))
  ->setBody('Here is the message itself')
  ;

$result = $mailer->send($message);
```

Dan anda boleh jalankannya melalui `php mail.php` dan email akan dihantar.

## Lock file
Jika anda perasan, setelah selesai menjalankan arahan `composer install`, satu fail baru bernama `composer.lock` akan dijana oleh `composer`. Fail ini berfungsi untuk merekodkan version sebenar *libraries* yang di'install' oleh `composer`. Ini kerana `composer.json` mungkin ditulis seperti berikut:-

```json
{
    "require": {
        "swiftmailer/swiftmailer": "5.3.1",
        "symfony/yaml": "~2.5"
    }
}
```
Dalam spesifikasi `composer.json` di atas, versi untuk `swiftmailer` ditulis secara spesifik manakala versi untuk `symfony/yaml` ditulis dalam format julat tertentu. `~2.5` adalah bersamaan dengan `>=2.5 < 3.0`. Bila `composer install` dijalankan buat pertama kali (`composer.lock` masih belum wujud), maka ia akan cuba mendapatkan versi terkini yang ada pada masa tersebut, mengikut julat yang ditetapkan.

Ini bermakna, jika developer A menjalankan `composer install` dan mendapat version `symfony/yaml 2.5.1` dan kemudian version baru dikeluarkan, developer B menjalankan `composer install` beberapa hari kemudian, dia mungkin akan mendapat version `2.5.2`. Ini menyebabkan perbezaan kod yang diperolehi oleh developer A dan B yang mungkin akan menyebabkan kekeliruan. Jadi `composer.lock` akan merekodkan version sebenar yang diperolehi oleh `composer` semasa ia pertama kali dijalankan dan `composer.install` seterusnya akan merujuk kepada spesifikasi yang terkandung dalam `composer.lock`.

Jadi amat penting sekali untuk `composer.lock` disertakan sekali ke dalam *version control system* yang anda gunakan.

## Rujukan
* https://getcomposer.org/doc/01-basic-usage.md#package-versions
* https://blog.engineyard.com/2014/composer-its-all-about-the-lock-file (terima kasih kepada [Mior Muhammad Zaki](https://github.com/crynobone)).
