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

```console
php composer.phar install
Loading composer repositories with package information
Installing dependencies (including require-dev
```
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
