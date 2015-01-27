---
layout: post
title: Langkah Pertama Menjadi Penyumbang di Github
author: kamalmustafa
date: 2015-01-27
level: 2
summary: >
            Bagaimana untuk mula menjadi penyumbang di Github dengan menguji
            patch atau pull-request daripada developer lain.
            
---

Ramai developer sekarang sudah mula menggunakan Github untuk menyimpan code
yang mereka hasilkan, ataupun mendapatkan code daripada developer lain. Namun fungsi
sebenar Github adalah untuk memudahkan kolaborasi ...

<!--more-->

Mulakan dengan *fork* repo yang anda berminat untuk menjadi penyumbang. Untuk tujuan
artikel ini, saya memilih projek Laravel di URL berikut:-

    https://github.com/laravel/laravel

Setelah proses *fork* selesai, saya akan mendapat salinan repo yang sama di URL:-

    https://github.com/k4ml/laravel

Langkah kedua adalah dengan *checkout* repo yang baru kita fork tadi:-

    git clone https://github.com/k4ml/laravel.git

Langkah kedua - add repo asal Laravel sebagai *upstream*:-

    cd laravel
    git remote add upstream https://github.com/laravel/laravel.git

Sekarang kita masuk ke bahagian yang ditunggu-tunggu. 'Menarik' *pull-request* oleh developer
lain dan mengujinya di komputer kita. Format arahan untuk tujuan ini adalah:-

    git fetch origin pull/<pull-request-id/head:<local-branch-name-to-pull>

Anda boleh dapatkan senarai *pull request* di url https://github.com/laravel/laravel/pulls.
Saya pilih [*pull request* bernombor 2821](https://github.com/laravel/laravel/pull/2821).

    git fetch upstream pull/2821/head:test-pr

Anda akan mendapat output lebih kurang berikut:-

    remote: Counting objects: 4, done.
    remote: Compressing objects: 100% (4/4), done.
    remote: Total 4 (delta 0), reused 3 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From https://github.com/laravel/laravel
     * [new branch]      refs/pull/2821/head -> test-pr

Untuk memastikan anda mendapat *changes* yang betul, jalankan command `git diff`:-

    git diff master

Bandingkan dengan *changes* yang anda lihat di https://github.com/laravel/laravel/pull/2821/files:-

<a href="http://i.imgur.com/P6BqGKu.png"><img src="http://i.imgur.com/P6BqGKul.png"></img></a>
