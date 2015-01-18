---
layout: post
title: 'Belajar Javascript: Bhg 1'
date: 2014-09-15
author: kamalmustafa
permalink: /belajar-javascript-bhg-1.html
---

Memetik kata-kata [Douglas Crockford][1], JavaScript adalah bahasa
pengaturcaraan yang sering disalahfahami walaupun ianya merupakan bahasa
pengaturcaraan yang paling popular sekali dengan penggunaan yang paling meluas.

<!--more-->

JavaScript, sebelum penggunaannya yang begitu meluas seperti sekarang biasanya
menjadi bahasa kelas kedua bagi kebanyakkan programmer. Saya katakan kelas
kedua kerana ia jarang dipelajari secara formal sepertimana bahasa lain seperti
PHP, Python, Ruby, Perl, Java, C dan sebagainya. Maksud 'formal' disini ialah
kita mengambil masa untuk berkenalan dengan bahasa tersebut bermula daripada
ciri-ciri asas seperti *data type*, *control structure* dan sebagainya.

Seringkali apabila terpaksa menggunakan JavaScript, kita akan mendapatkan
library ataupun *code snippet* di Internet, ubah beberapa baris dan sekiranya
ia melakukan apa yang kita kehendaki, selesai ! Akhirnya JavaScript sering
menjadi cercaan apabila beberapa masalahnya yang tidak dijangka kita temui
dalam aplikasi yang kita bangunkan.

Saya bercadang untuk mula mempelajari JavaScript secara lebih tersusun dan
berharap dapat berkongsi pengalaman tersebut melalui beberapa siri tulisan
dalam blog ini. Untuk proses pembelajaran ini, saya akan cuba membina sebuah
aplikasi JavaScript ringkas dan akan cuba meneroka ciri-ciri asas JavaScript.

Ini bagi saya lebih menarik dan tidak menjemukan berbanding mencuba satu demi
satu contoh kod bagi setiap *features* yang ada. Sebaliknya kita akan
mengenalpasti masalah yang perlu diselesaikan dan cuba cari *features*
JavaScript yang boleh digunakan untuk menyelesaikan masalah tersebut.

Aplikasi yang saya ingin bangunkan adalah fungsi *autocomplete* ringkas. Kita
selalu temui *features* ini dalam banyak laman web, terutamanya yang melibatkan
fungsi carian. Saya juga banyak menggunakan *autocomplete* dalam aplikasi yang
saya bangunkan. Namun sehingga ke hari ini saya tidak pernah mengambil tahu
bagaimana sebenarya fungsi autocomplete ini berfungsi dalam JavaScript.

Kita mulakan aplikasi ini dengan kod html ringkas seperti berikut:-

```html
<html>
<head>
<script src="app.js"></script>
</head>
<body>
<input type="text" name="keyword" id="keyword" value="" size="20" />
</body>
</html>
```

`app.js` pula akan kelihatan seperti di bawah:-

```js
(function() {
    var keyword = document.getElementById('keyword');
    alert (keyword);
}());
```

Daripada kod seringkas ini pun sebenarnya banyak yang dapat dipelajari
berkaitan JavaScript. Pertama sekali adalah cara kod itu sendiri ditulis. Ia
mungkin sedikit pelik bagi yang telah biasa menulis kod aturcara dalam bahasa
pengaturcaraan lain seperti PHP, Python, Perl, Java atau C. Sebenarnya kod
JavaScript digalakkan ditulis dalam bentuk sedemikian rupa untuk mengelakkan
*variable-variable* yang digunakan daripada bocor (*leaked*) ke dalam skop
global program. Ini antara satu kekurangan JavaScript dimana semua unit
aturcara hanya boleh wujud dalam satu skop iaitu global. Tidak wujud *module*
atau *namespace* dalam JavaScript. Bagaimanapun kita agak bernasib baik kerana
*function* dalam JavaScript adalah agak fleksibel jadi kita boleh
menggunakannya untuk mengehadkan skop variable yang kita gunakan.

Walaupun *function* dalam JavaScript boleh digunakan untuk mengehadkan skop,
masih terdapat satu lagi keburukan JavaScript yang mesti diambil perhatian oleh
semua programmer iaitu kesemua variable yang digunakan dalam *function* mesti
diisytiharkan menggunakan *keyword* `var` sepertimana yang kita lihat dalam
contoh kod di atas. Jika tidak, ia akan turut wujud dalam skop global walaupun
hanya digunakan dalam *function* ! Sebagai contoh, perhatikan kod di bawah:-

```js
function add(num1, num2) {
    _tmp1 = parseInt(num1);
    _tmp2 = parseInt(num2);

    return _tmp1 + _tmp2;
}

total = add(1, 2);
console.log(_tmp1);
```

Dalam kod di atas, `tmp1` dan `_tmp2` hanyalah *variable* sementara dan
sepatutnya wujud dalam function `add` sahaja. Namun anda akan dapati
`console.log` tetap memaparkan nilai 2 iaitu nilai `_tmp2` di dalam function
`add` ! Ini tidak sepatutnya berlaku kerana dalam satu aturcara yang besar, ia
akan menyebabkan bug yang sukar dijejaki di mana puncanya kerana variable
`_tmp1` kini boleh dicapai oleh mana-mana bahagian aturcara sekalipun. Untuk
membetulkan keadaan di atas, *keyword* `var` mesti sentiasa digunakan untuk
mengisytiharkan *variable* dalam *function*. Contoh:-

```js
function add(num1, num2) {
    var _tmp1 = parseInt(num1);
    var _tmp2 = parseInt(num2);

    return _tmp1 + _tmp2;
}
```

Seterusnya mengapa contoh kod sebelum ini ditulis dalam bentuk function ? Ini
juga melibatkan isu berkaitan global variable dalam JavaScript. Untuk
meminimumkan bilangan *variable* yang didedahkan kepada skop global, kita
*wrap* kod tersebut dalam function yang terus dipanggil apabila fail tersebut
dibuka oleh *JavaScript engine*. Kod sebelum ini contohnya, tidak mendedahkan
sebarang variable kepada skop global berbanding sekiranya ia ditulis seperti
berikut:-

```js
function init() {
    var keyword = document.getElementById('keyword');
    alert (keyword);
}

init();
```

Dalam contoh ini kita telah mendedahkan satu nama baru ke dalam skop global
iaitu `init` walaupun `init` mungkin hanya akan digunakan sekali iaitu untuk
*run* kod dalam function tersebut. Jika kita perhatikan library JavaScript yang
besar seperti JQuery, YUI, Backbone dan sebagainya mereka hanya mendedahkan
satu nama ke dalam skop global seperti jQuery/$ utk JQuery dan YUI untuk YUI.  *Function-function* lain kesemuanya diakses melalui *top level* namespace
tersebut seperti `$.getJSON`, `YUI.dom` dan sebagainya. Walaupun JavaScript
tidak mempunyai sokongan *namespace* atau *module*, function dan object boleh
digunakan untuk *simulate* namespace. Lagi yang boleh dipelajari daripada contoh
ringkas ini adalah perbezaan antara *function declaration* dan *function expression* tetapi saya tidak bercadang untuk mengulasnya dalam bahagian ini.

Setakat ini sahaja untuk bahagian pertama. Saya berharap akan dapat terus menulis dan berkongsi bahagian seterusnya, Insya Allah.

[1]:http://javascript.crockford.com/
[2]:http://stackoverflow.com/questions/1634268/explain-javascripts-encapsulated-anonymous-function-syntax
[3]:http://stackoverflow.com/questions/9342122/javascript-on-load-execution
[4]:https://plus.google.com/104286962752255423480/posts
[5]:https://plus.google.com/u/0/104286962752255423480/posts/Tb1ffbfzZdM
