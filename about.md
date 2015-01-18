---
layout: page
title: Mengenai Kami
---

<p class="message">
  Maklumat di halaman ini belum dikemaskini sepenuhnya.
</p>
## Tentang MyDev

MyDev adalah komuniti pembangun perisian dan web di Malaysia. Anda dijemput untuk menyertai kami dan menyumbang dalam perbincangan sekitar topik pembangunan perisian dan web, teknologi maklumat, kerjaya, dan sebagainya. Atau paling tidak boleh melepak dengan geng-geng sekepala :)

Sertai kami di [MyDev Google+ Community](https://plus.google.com/communities/104883828501447858589)


## Tentang Laman Kolaborasi MyDev

Ini adalah satu inisiatif MyDev untuk memperkayakan sumber maklumat dalam Bahasa Melayu tentang pembangunan perisian dan web. Laman ini diinspirasikan dari [laman html5rocks](http://www.html5rocks.com/) yang mana kandungannya adalah disumbangkan oleh orang ramai. Semua kandungan termasuk kod sumber untuk laman ini kami letakkan di dalam [GitHub](http://github.org). Orang ramai boleh menambah atau mengubah artikel melalui perisian Git version control system.


## Ingin Menyumbang?

Kami menjemput sumbangan artikel berkaitan pembangunan perisian dan web dari semua. Syaratnya cuma satu, artikel perlu di dalam Bahasa Melayu. Artikel boleh jadi hasil asli ataupun terjemahan artikel dari bahasa lain (sila hormati hakcipta jika ada). Anda juga digalakkan melakukan pembetulan untuk artikel sedia ada.

Anda perlu tahu menggunakan [Markdown](http://daringfireball.net/projects/markdown/syntax) untuk menulis artikel. Anda juga perlu tahu bagaimana menggunakan [Git](http://git-scm.com) dan melakukan pull request dalam [GitHub](http://github.org). Kami menggunakan [Pelican](http://getpelican.com) sebagai CMS, jadi mungkin anda perlu tahu serba sedikit mengenainya juga.

Secara umumnya, langkah untuk menyumbang adalah seperti berikut:

1. _Fork repository_ [https://github.com/mydevcommunity/mydev](https://github.com/mydevcommunity/mydev).
1. Lakukan proses menulis atau mengedit. Buat satu fail dalam _folder_ `site\content` atau ubah fail sedia ada. Lihat artikel lain sebagai contoh atau rujuk panduan di [sini](http://docs.getpelican.com/en/3.1.1/getting_started.html#writing-articles-using-pelican).
1. Bila selesai, hantarkan satu _pull request_ ke _repository_ asal.
1. Admin akan menilai _pull request_ anda. Ia kemudiannya akan _dipublish_ ke laman ini.

Untuk mengedit (proses 2. di atas), anda boleh `git clone` ke komputer anda dan gunakan editor kegemaran anda. Anda juga boleh menggunakan fitur di dalam GitHub sendiri untuk mengedit. Selain itu anda boleh menggunakan _online IDE_ seperti [Cloud9](http://c9.io).

Jika anda memerlukan editor WYSIWYG untuk Markdown, salah satu _online editor_ yang boleh dicuba ialah [Dillinger.io](http://dillinger.io/)

Jika anda ingin membuat perubahan terhadap _theme_ atau perkara lain yang bukan _content_. Anda mungkin perlu _generate_ laman ini sendiri di komputer anda untuk memastikan semuanya baik sebelum melakukan pull request. Caranya ialah:

* Pastikan Python sudah diinstall. Dapatkan Python dari distribusi rasmi [Python.org](http://python.org/download/) atau lain-lain seperti [Portable Python](http://portablepython.com/).
* Larikan arahan ini pada terminal/command line:-

```console
mkdir -p app-root && ln -s ../ app-root/repo
OPENSHIFT_HOMEDIR=. OPENSHIFT_DATA_DIR=.env .openshift/action_hooks/build
PORT=9090 IP=127.0.0.1 python server.py
```

* Anda boleh buka akses melalui browser http://127.0.0.1:9090/

Jika anda menggunakan Cloud9, arahan sama boleh dilarikan di terminal dalam workspace Cloud9 anda. Cloud9 sudah terbina dalam dengan Python dan Git.

Dengan menyumbang ke laman ini bermakna anda menerima untuk melesenkan artikel anda dibawah [Creative Commons Attribution 4.0 License](http://creativecommons.org/licenses/by/4.0/).

## Kenapa Git dan GitHub?

Kami letakkan kandungan dan kod sumber laman ini di GitHub supaya jika ditakdirkan suatu hari nanti laman ini tidak dapat diteruskan, semua ilmu yang telah dikongsikan tidak terpendam begitu sahaja. Ia masih dapat diperolehi dari GitHub dan dari _clone_ yang ada pada orang ramai, lalu masih dapat dimanfaatkan oleh semua.

Melalui _pull request_ dalam GitHub juga boleh digunakan sebagai suatu sistem penilaian yang _ad-hoc_ oleh admin untuk mengawal kualiti laman ini.

Lagi pula, pada yang orang-orang baru, melalui cara ini mereka dapat belajar teknik-teknik menggunakan _distributed version control system_ dan berkolaborasi secara online.

