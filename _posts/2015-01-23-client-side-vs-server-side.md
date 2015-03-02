---
layout: post
title: Client-Side vs Server-Side
author: ikhwanhayat
level: 1
include_toc: true
summary: >
            Perbezaan asas antara client-side dan server-side dalam web development. Bacaan wajib bagi
            mereka yang baru bermula dalam bidang ini, ataupun masih keliru dengan perbezaan antara
            keduanya.
---

Suatu sistem berasaskan web adalah komunikasi antara 2 pihak. Pihak pertama ialah pelayar web iaitu yang kita panggil **_client-side_**. Pihak kedua pula ialah pelayan ataupun **_server-side_**. _Client_ akan memulakan proses komunikasi dengan meminta fail dari _server_, dan _server_ akan memulangkan fail tersebut kepada _client_. _Client_ akan memproses fail tersebut dan memaparkannya kepada pengguna. Seorang pengaturcara web perlu mempunyai pemahaman yang baik tentang konsep ini.

<!--more-->

## Bahasa Client-Side

Fail yang diterima pada client biasanya adalah dalam bahasa **HTML**. Kandungan yang ingin dipaparkan seperti teks, imej, jadual, dan lain-lain ditetapkan melalui HTML. Satu bahasa lain pula iaitu **CSS** digunakan untuk membantu menggayakan elemen yang terdapat dalam HTML tadi, seperti mewarnakan teks atau latar belakang, menetapkan tebal garisan, dan sebagainya. HTML dan CSS dikatakan sebagai bahasa pengaturcaraan _client-side_.

Di bawah ialah contoh mudah kandungan sebuah fail HTML yang mengandungi sedikit CSS. Ia akan memaparkan sebuah jadual:

```html
<html>
  <head>
    <style rel="stylesheet" type="text/css">
    /*
     * CSS untuk menetapkan gaya tulisan dan jadual
     */
    div.title {
      font-size: 130%;
    }
    table {
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid black;
      padding: 5px;
    }
    </style>
  </head>
  <body>
    <!--
      Kandungan atau maklumat yang ingin dipaparkan
    -->
    <div class="title">Density of Precious Metals, in g/cm^3</div>
    <table>
      <tr>
        <th>Element</th>
        <th>Density</th>
      </tr>
      <tr>
        <td>Copper</td>
        <td>8.94</td>
      </tr>
      <tr>
        <td>Silver</td>
        <td>10.49</td>
      </tr>
      <tr>
        <td>Gold</td>
        <td>19.30</td>
      </tr>
      <tr>
        <td>Platinum</td>
        <td>21.45</td>
      </tr>
    </table>
  </body>
</html>
```

Jadual seperti di bawah ini akan dipaparkan pada pelayar web:

![Result 1]({{ site.baseurl }}/public/client-side-vs-server-side/diag1.jpg)

## Bahasa Server-Side

Fail HTML ini mungkin sahaja adalah sememangnya sebuah fail sebenar yang berada di _server_. Sang pengaturcara menaipkan kod-kod HTML dan CSS seperti di atas ke dalam sebuah editor teks dan menyimpannya sebagai sebuah fail. Fail itu kemudiannya diletakkan di dalam satu folder di _server_ yang boleh diakses oleh _client_ atau pelayar web. Fail seperti ini kadangkala dipanggil _static HTML_.

Namun boleh jadi juga fail HTML ini tidak wujud terlebih dahulu, tetapi dijanakan secara dinamik. Sang pengaturcara boleh menggunakan bahasa seperti **PHP, Java, C#, Ruby, Python, Perl** dan sebagainya untuk menjana HTML tersebut. Bahasa-bahasa seperti ini disebut bahasa pengaturcaraan _server-side_.

Berikut adalah contoh bagaimana bahasa PHP boleh digunakan untuk menghasilkan output yang seperti di atas, tetapi dengan datanya diambil dari sebuah pangkalan data berbanding ditaip terus seperti tadi.

```php
<html>
  <head>
    <style rel="stylesheet" type="text/css">
    /*
     * CSS untuk menetapkan gaya tulisan dan jadual
     */
    div.title {
      font-size: 130%;
    }
    table {
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid black;
      padding: 5px;
    }
    </style>
  </head>
  <body>
    <!--
      Kandungan atau maklumat yang ingin dipaparkan
    -->
    <div class="title">Density of Precious Metals, in g/cm^3</div>
    <table>
      <tr>
        <th>Element</th>
        <th>Density</th>
      </tr>
      <?php
        /*
         * Dapatkan data dari pangkalan data.
         * Setiap "metal" mengandungi maklumat
         * "name" dan "density".
         */
        $metals = retrieveMetalDensitiesFromDatabase();

        foreach ($metals as $metal)
        {
      ?>
      <tr>
        <td><?php echo $metal["name"]; ?></td>
        <td><?php echo $metal["density"]; ?></td>
      </tr>
      <?php } ?>
    </table>
  </body>
</html>
```

Ia akan menghasilkan output sama seperti di atas, jika kandungan pangkalan data itu mengandungi senarai yang sama.

## JavaScript

Berbalik kepada _client-side_, ada satu lagi bahasa yang kita boleh gunakan iaitu **JavaScript**. Ia boleh menjadikan halaman web lebih dinamik, contohnya seperti menggerakkan teks ke kiri dan ke kanan, mengeluarkan dialog amaran, menukar warna latar belakang mengikut masa dan lain-lain. Secara mudahnya, boleh dikatakan JavaScript ini memanipulasi HTML dan CSS tadi untuk memaparkan kandungan dan gaya berbeza kepada pengguna.

Contoh di bawah ialah kod JavaScript yang memaparkan waktu semasa. Ia berdetik setiap saat and mengemaskini waktu yang dipaparkan.

```html
<html>
  <body>
    Time now is <span id="clock"></span>.
    <script>
    function displayTime() {
      document.getElementById("clock").innerHTML = new Date();
    }
    setInterval(displayTime, 1000);
    displayTime();
    </script>
  </body>
</html>
```

<div class="message">
  Time now is <span id="csvsss_clock"></span>.
</div>

<script>
function displayTime() {
  document.getElementById("csvsss_clock").innerHTML = new Date();
}
setInterval(displayTime, 1000);
displayTime();
</script>

Penggunaan JavaScript pada masa kini sudah menjadi sangat maju sehingga pelbagai perkara dapat dibuat untuk menjadikan paparan web lebih menarik. Ada banyak kod JavaScript yang siap digunakan di luar sana.

Di sini ingin dibawakan satu contoh kod JavaScript yang dinamakan Google Charts yang mana boleh digunakan untuk membina perlbagai graf dan carta. Kita cuba masukkan satu carta lajur untuk jadual kita di atas.

```html
<html>
  <head>
    <style rel="stylesheet" type="text/css">
    /*
     * CSS untuk menetapkan gaya tulisan dan jadual
     */
    div.title {
      font-size: 130%;
    }
    table {
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid black;
      padding: 5px;
    }
    </style>
  </head>
  <body>
    <!--
      Kandungan atau maklumat yang ingin dipaparkan
    -->
    <div class="title">Density of Precious Metals, in g/cm^3</div>
    <table>
      <tr>
        <th>Element</th>
        <th>Density</th>
      </tr>
      <tr>
        <td>Copper</td>
        <td>8.94</td>
      </tr>
      <tr>
        <td>Silver</td>
        <td>10.49</td>
      </tr>
      <tr>
        <td>Gold</td>
        <td>19.30</td>
      </tr>
      <tr>
        <td>Platinum</td>
        <td>21.45</td>
      </tr>
    </table>

    <!--
      JavaScript untuk membina carta
    -->
    <div id="chart"></div>
    <script type="text/javascript"
            src="https://www.google.com/jsapi"></script>
      <script type="text/javascript">
        google.load('visualization', '1.0',
                    {'packages':['corechart']});
        google.setOnLoadCallback(drawChart);
        function drawChart() {
          var data = google.visualization.arrayToDataTable([
            ['Element', 'Density'],
            ['Copper', 8.94],
            ['Silver', 10.49],
            ['Gold', 19.30],
            ['Platinum', 21.45]
          ]);
          var chart = new google.visualization.ColumnChart(document.getElementById('chart'));
          chart.draw(data);
        }
      </script>
  </body>
</html>
```

Hasilnya akan kelihatan seperti berikut:

![Result 2]({{ site.baseurl }}/public/client-side-vs-server-side/diag2.jpg)

## Kerjasama Bahasa Client-Side dan Server-Side

Jika kita ada menggunakan bahasa _server-side_, kita boleh mendapatkan hasil yang sama melalui kod seperti berikut:

```php
<html>
  <head>
    <style rel="stylesheet" type="text/css">
    /*
     * CSS untuk menetapkan gaya tulisan dan jadual
     */
    div.title {
      font-size: 130%;
    }
    table {
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid black;
      padding: 5px;
    }
    </style>
  </head>
  <body>
    <!--
      Kandungan atau maklumat yang ingin dipaparkan
    -->
    <div class="title">Density of Precious Metals, in g/cm^3</div>
    <table>
      <tr>
        <th>Element</th>
        <th>Density</th>
      </tr>
      <?php
        /*
         * Dapatkan data dari pangkalan data.
         * Setiap "metal" mengandungi maklumat
         * "name" dan "density".
         */
        $metals = retrieveMetalDensitiesFromDatabase();

        foreach ($metals as $metal)
        {
      ?>
      <tr>
        <td><?php echo $metal["name"]; ?></td>
        <td><?php echo $metal["density"]; ?></td>
      </tr>
      <?php } ?>
    </table>

    <!--
      JavaScript untuk membina carta
    -->
    <div id="chart"></div>
    <script type="text/javascript"
            src="https://www.google.com/jsapi"></script>
      <script type="text/javascript">
        google.load('visualization', '1.0',
                {'packages':['corechart']});
        google.setOnLoadCallback(drawChart);
        function drawChart() {
          var data = google.visualization.arrayToDataTable([
          <?php
            /*
             * Gunakan data "metals" tadi untuk
             * membina carta pula.
             */
            foreach ($metals as $metal)
            {
              echo "['".$metal["name"]."', '".$metal["density"]."'],";
            }
          ?>
          ]);
          var chart = new google.visualization.ColumnChart(document.getElementById('chart'));
          chart.draw(data);
        }
      </script>
  </body>
</html>
```

Perhatikan, bagaimana dengan bahasa _server-side_ (PHP), kita membina output yang terdiri dari bahasa _client-side_ (sama ada HTML, CSS, atau JavaScript). Hasilnya pula kemudiannya diproses oleh pelayar web dan dipaparkan. Ia seperti lapisan bahasa pengaturcaraan di atas bahasa pengaturcaraan yang lain!

Perhatikan juga bagaimana di dalam satu fail berselang-seli bahasa _server-side_ dan _client-side_. Perkara ini kadang-kala mengelirukan pengaturcara-pengaturcara baru. Jika anda menghadapi situasi ini, cuba kembali kepada asasnya. Kenalpasti yang mana adalah kod _client-side_ dan yang mana _server-side_. Pemahaman yang baik tentang asas ini membolehkan anda cekap bermain dengan kod _client-side_ dan _server-side_ dan dapat menghasilkan penyelesaian yang kreatif.
