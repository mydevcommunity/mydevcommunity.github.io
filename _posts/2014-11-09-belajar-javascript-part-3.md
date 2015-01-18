---
layout: post
title: 'Belajar Javascript: Bhg 3'
date: 2014-11-09
author: kamalmustafa
permalink: /belajar-javascript-bhg-3.html
---

Alhamdulillah, kita dapat bertemu lagi dalam bahagian ke-3, siri belajar bahasa
pengaturcaraan JavaScript. Untuk bahagian ke-3 ini bagaimanapun saya memohon maaf
terlebih dahulu kerana fokusnya bukan pada JavaScript secara umum tetapi lebih
kepada 'browser DOM programming' kerana saya ingin memulakan apa yang saya rancang
pada bahagian 1 iaitu membina sebuah *autocomplete* ringkas.

<!--more-->

Kita mulakan dengan dokumen HTML ringkas seperti di bawah:-

```html
<html>
<head>
<style>
</style>
</head>
<body>
<input type="text" name="keyword" id="keyword" value="" size="20" />
<script src="app.js"></script>
</body>
</html>
```
Manakala `app.js` adalah seperti berikut:-

```js
(function() {
    var keyword_elm = document.getElementById('keyword');
}());
```
!!! info "Nota" "info-sign"
    Saya akan menggunakan *suffix* `_elm` bagi setiap *variable* dalam tutorial ini
    bagi menunjukkan yang ia adalah *DOM element*.

Sebelum itu mari kita lihat dulu perkara-perkara yang perlu kita lakukan untuk membina
fungsi *auto complete* ini:-

* Apabila pengguna menaip beberapa karakter di dalam kotak carian, satu *pop-up* akan
  dipaparkan di bawah kotak carian tersebut, memaparkan beberapa cadangan yang berkaitan.
* Pengguna boleh klik mana-mana cadangan yang dipaparkan dan ia akan dimasukkan ke dalam
  kotak carian.
* Pengguna juga boleh skroll *pop-up* cadangan dan memilih hanya dengan menekan butang
  Enter pada *keyboard*.

Bagi keperluan pertama, kita boleh menggunakan *event* `keyup` pada elemen input kotak
carian. *Event* `keyup` ini akan dijana oleh browser apabila pengguna menekan sesuatu
kekunci pada *keyboard* dan melepaskannya. Jadi kita boleh menulis kod JavaScript seperti
berikut:-

```js
(function() {
    var keyword_elm = document.getElementById('keyword');
    keyword_elm.addEventListener('keyup', function(evt) {
        var keyword = evt.target.value;
        console.log(keyword);
    });
}());
```

!!! warning "Nota" "exclamation-sign"
    Method `.addEventListener()` tidak disokong oleh IE7 dan IE6. Kebanyakkan contoh dalam
    tutorial ini menganggap anda menggunakan browser moden yang terkini.

Setiap *event handler function* akan di*pass* satu *argument* berbentuk *event object*
yang mengandungi maklumat terperinci berkaitan *event* yang dijana oleh browser.
Maklumat lanjut berkaitan *event handler* ini boleh dirujuk kepada laman [Eloquent
JavaScript][eloquent]. Melalui *event object* ini, kita akan dapat merujuk kembali
kepada elemen asal di mana *event* tersebut dijana. Ini membolehkan kita untuk meraih
nilai yang dimasukkan oleh pengguna pada elemen input kotak carian.

*Event* `keyup` akan dijana oleh browser setiap kali pengguna menaip sesuatu karakter
pada *keyboard* dan ini juga bermakna setiap kali itulah juga kod JavaScript kita
perlu *handle* dan memproses *event* tersebut. Ia agak tidak efisien dan antara cadangan
yang saya jumpa adalah dengan menggunakan function `setTimeout` untuk [mengesan hanya
setelah pengguna berhenti menaip baru kod JavaScript kita bertindak][settimeout]. Saya
bagaimanapun akan mengabaikan isu ini buat seketika.

Kita teruskan dengan melaksanakan apa yang diperlukan pada syarat pertama fungsi ini
iaitu memaparkan *pop-up* apabila pengguna menaip sesuatu pada kotak carian. Untuk
permulaan ini, saya akan menggunakan cara yang paling naif terlebih dahulu. Kita akan
sama-sama menambah baik menggunakan teknik yang lebih sesuai dan canggih pada siri yang
akan datang, setelah kita semakin menguasai teknik pengaturcaraan `browser DOM`
menggunakan JavaScript ini.

Cara naif pertama yang saya gunakan adalah dengan meletakkan *hidden div* di bawah
kotak carian untuk dipaparkan sebagai *pop-up*:-

```html
<html>
<head>
<style>
#keyword-result {
    visibility: hidden;
    border: 1px solid;
    width: 170px;
}
</style>
</head>
<body>
<input type="text" name="keyword" id="keyword" value="" size="20" />
<div id="keyword-result">
</div>
<script src="app.js"></script>
</body>
</html>
```

Kod JavaScript adalah seperti berikut:-

```js
(function() {
    var keyword_elm = document.getElementById('keyword');
    keyword_elm.addEventListener('keyup', function(evt) {
        var keyword = evt.target.value;
        var keyword_result_elm = document.getElementById('keyword-result');
        var list_elm = document.createElement('ul');
        if (keyword.length > 2) {
            for (var i = 0; i < 5; i++) {
                var list_item_elm = document.createElement('li');
                var list_item = document.createTextNode('result ' + i);
                list_item_elm.appendChild(list_item);
                list_elm.appendChild(list_item_elm);
            }
        }
        keyword_result_elm.appendChild(list_elm);
        keyword_result_elm.style.visibility = 'visible';
    });
}());
```

Dalam kod di atas, apa yang ia lakukan adalah bertindak balas terhadap *event* `keyup`
pada kotak carian dan kemudian secara dinamik membina sebuah *unordered list* (`<ul>`).
List tersebut kemudian dimasukkan ke dalam elemen `div` yang pada asalnya adalah *hidden*.
Elemen `div` tersebut kemudian dipaparkan dengan mengeset *properties* *visibility* kepada
`visible`, memberikan efek seperti element tersebut *pop-up* pada bahagian bawah kotak
carian.

Setelah dapat memaparkan *pop-up*, kita beralih kepada syarat kedua iaitu membolehkan
setiap elemen dalam *list* yang kita jana sebelum ini boleh di'klik' dan nilainya
dipaparkan pada kotak carian. Ianya boleh dilakukan dengan meng'attach' *event* `click`
pada setiap item dalam list tersebut:-

```javascript
list_item_elm.addEventListener('click', function(evt) {
    keyword_elm.value = evt.target.textContent;
    keyword_result_elm.style.display = 'none';
});
```
Di sini kita raih nilai string yang ada pada setiap item dalam list dan setkan sebagai
*value* kepada kotak carian. Seterusnya kita set *properties* *display* pada *pop-up*
kepada `none` untuk memberikan kesan seperti ia hilang.

Syarat ketiga adalah untuk membolehkan pengguna memilih menggunakan *keyboard* berbanding
mengklik menggunakan *mouse*. Saya masih belum dapat mencari penyelesaian kepada masalah
ini. Cubaan pertama saya adalah dengan *properties* `scroll` pada `div`:-

```css
#keyword-result {
    display:hidden;
    border: 1px solid;
    width: 170px;
    overflow-y: scroll;
}
```
Ia akan meletakkan *scroll bar* pada `div` tersebut tetapi kita masih belum dapat skroll
seperti fungsi *auto complete* yang biasa kita temui. Ia mungkin sebab *focus* masih
pada kotak carian. Jadi saya cuba set *fokus* pada *pop-up* tersebut tetapi ini akan
menghalang saya daripada terus menaip pada kotak carian kerana kini *focus* adalah pada
*pop-up*. Ini adalah satu masalah yang menarik, saya masih lagi meneliti [script ini][autoComplt]
dan [ini][completely] untuk mengenalpasti bagaimana ia menyelesaikan masalah *scroll* ini.

Sekiranya anda mempunyai sebarang idea, saya mengalu-alukan idea dan cadangan anda ! Kod penuh untuk
tutorial ini boleh dilihat pada laman [codepen].

[eloquent]:http://eloquentjavascript.net/14_event.html
[settimeout]:http://davidwalsh.name/javascript-settimeout
[autoComplt]:https://github.com/Fischer-L/autoComplt
[completely]:http://complete-ly.appspot.com/
[codepen]:http://codepen.io/k4ml/pen/zxxzJq

### Rujukan
* http://stackoverflow.com/questions/9707397/making-a-div-vertically-scrollable-using-css
* http://stackoverflow.com/questions/6754275/set-keyboard-focus-to-a-div
