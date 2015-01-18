---
layout: post
title: 'Belajar Javascript: Bhg 2'
date: 2014-11-04
author: kamalmustafa
permalink: /belajar-javascript-bhg-2.html
---

Sebelum ini kita telah menulis kod JavaScript asas seperti berikut:-

```js
(function() {
    var keyword = document.getElementById('keyword');
    alert keyword;
})();
```

Bagi yang biasa dengan JavaScript pasti menyedari ada masalah dengan kod di
atas. Malah jika anda membuka fail `index.html` melalui browser, anda akan
dapati nilai yang dipaparkan dalam `alert` adalah `null`. Ini sudah pasti bukan
yang kita harapkan kerana nilai yang sepatutnya adalah reference kepada DOM
object HTMLInput. Ini adalah disebabkan kod tersebut terus dijalankan apabila
ia dibaca dalam browser. Bagaimanapun sentiasa ada kemungkinan semasa kod
tersebut dijalankan, DOM element yang kita cuba dapatkan masih belum disediakan
sepenuhnya oleh browser.

<!--more-->

Untuk membetulkan masalah di atas, kita perlu attach function tersebut kepada
`load` event sama ada pada object `window` ataupun pada element `body`. Contohnya adalah seperti berikut:-

```js
window.onload = function() {
    var keyword = document.getElementById('keyword');
    alert('using window.onload');
    alert (keyword);
}
```

Atau:-

```js
function init() {
    var keyword = document.getElementById('keyword');
    alert('using body.onload');
    alert (keyword);
}
```

```html
// dalam fail index.html
<body onload="init()">
</body>
```

Tidak ada DOM object untuk `body` dan saya pada mulanya mencuba seperti
berikut:-

```js
document.body.onload = function() {
    var keyword = document.getElementById('keyword');
    alert('using document.body.onload');
    alert (keyword);
}
```

tetapi mendapat error `Uncaught TypeError: Cannot set property 'onload' of
null`. Menggunakan `body onload=init()` bagaimanapun memerlukan untuk kita
declare satu function pada skop global, sesuatu yang kita cuba elakkan seperti
yang telah dibincangkan dalam tulisan yang lalu. Bagi browser moden pada hari
ini, cara yang direkomenkan adalah dengan menggunakan event listener seperti
berikut:-

```js
(function() {
    window.addEventListener('load', function() {
    var keyword = document.getElementById('keyword');
    alert (keyword);
    }, false);
}());
```

Kelebihan cara di atas adalah struktur kod kita masih kekal sebagaimana asal
dipermulaan siri ini. Bagaimanapun menggunakan `load` event tetap mempunyai
satu masalah iaitu kod tersebut hanya akan dijalankan apabila kesemua elemen
dan juga *resource* seperti imej telah selesai dimuat-turun oleh browser. Kebanyakkan kod JavaScript adalah untuk memanipulasi DOM jadi agak membuang
masa dan juga mungkin menghasilkan kesan yang tidak diingini jika terpaksa
menunggu kesemua *resource* selesai dimuat-turun sebelum kod JavaScript kita
boleh memainkan peranan. Alternatif kepada `load` event adalah
`DOMContentLoaded` dan kod di atas boleh ditulis seperti berikut:-

```js
(function() {
    window.addEventListener('DOMContentLoaded', function() {
    var keyword = document.getElementById('keyword');
    alert (keyword);
    }, false);
}());
```

Kelebihan `DOMContentLoaded` adalah ia akan terus *execute* kod kita sebaik
sahaja kesemua struktur DOM telah dibina dalam memori. Namun hidup dalam dunia
JavaScript adalah sangat tidak menentu dan sukar diduga. Tidak semua browser
menyokong event `DOMContentLoaded` ini jadi kod kita perlu melakukan beberapa
adaptasi bagi membolehkan ia berfungsi pada semua browser. Atas sebab inilah
library seperti JQuery menyediakan function khas untuk mengatasi masalah ini. Menggunakan JQuery, kod di atas boleh ditulis seperti berikut:-

```js
(function() {
    $(document).ready(function() {
        var keyword = document.getElementById('keyword');
        alert (keyword);
    });
}());
```

Untuk membaca dengan lebih lanjut berkaitan isu yang dibincangkan dalam
bahagian ini boleh rujuk perbincangan di laman stackoverflow:-

1. http://stackoverflow.com/questions/3698200/window-onload-vs-document-ready
1. http://stackoverflow.com/questions/3474037/window-onload-vs-body-onload-vs-document-onready

## Nota
Saudara [Zulfa Juniadi][zulfa] memberikan komen dalam grup FB [JomWeb] bahawa penggunaan
`$.ready()` adalah tidak perlu sekiranya kod JavaScript kita diletakkan pada pengakhiran dokumen, sebelum tag `</body>`. Ini
kerana apabila kod tersebut dijalankan oleh browser, kesemua kandungan DOM telah pun diproses
oleh browser, menghasilkan kesan yang sama seperti `DOMContentLoaded` yang dibincangkan di atas.
Perbincangan berkaitan di Stackoverflow - http://stackoverflow.com/q/4643990/139870.

[Cadangan lain] di Stackoverflow adalah untuk load JavaScript pada bahagian `<head>` tapi menggunakan `async` atau `defer`
attribute. Cara ini juga tidak akan *block* browser daripada terus memproses laman HTML kita dan disokong oleh
80% browser moden pada hari ini.

Sekian untuk kali ini, sehingga berjumpa lagi untuk siri akan datang, Insya
Allah.

[zulfa]:https://github.com/zulfajuniadi
[JomWeb]:https://www.facebook.com/groups/jomweb/
[Cadangan lain]:http://stackoverflow.com/questions/436411/where-is-the-best-place-to-put-script-tags-in-html-markup/24070373#24070373
