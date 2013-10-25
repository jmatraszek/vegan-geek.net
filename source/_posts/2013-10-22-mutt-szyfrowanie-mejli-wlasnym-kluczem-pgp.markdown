---
layout: post
title: "Mutt: szyfrowanie mejli własnym kluczem PGP"
date: 2013-10-22 21:45
comments: true
categories: Linux
---
O kliencie pocztowym pod nazwą mutt miałem napisać już wiele razy. Nigdy nie udało mi się za to zabrać. Dziś napiszę
 szybkiego posta o takim szyfrowaniu mejli, żebyśmy byli potem w stanie odczytać nasze własne wiadomości...

{% imgcap /images/post_images/mutt_gpg.jpg Zaszyfrowana wiadomość wysłana przeze mnie. %}

<!--more-->

Setup PGP/GPG w przypadku mutta nie jest wielkim problemem -- w internecie jest dużo przykładów. Ja znalazłem gdzieś
plik konfiguracyjny, wkleiłem go -- zadziałało. Miałem tylko jeden problem -- nie mogłem odczytać wysłanych przeze mnie
mejli, które zostały zaszyfrowane. Mutt wyświetlał nieco enigmatyczny komunikat: "Could not copy message".

Było to całkiem zrozumiałe -- tekst wiadomości został zaszyfrowany przy użyciu klucza publicznego osoby, do której
pisałem. Ja nie posiadałem pasującego klucza prywatnego, więc to nawet dobrze, że nie mogłem odczytać tej wiadomości...
ale w innych programach pocztowych to działało.

Rozwiązań chyba jest kilka, chociaż ja wybrałem najlepsze (w mojej ocenie). Polega ono na zaszyfrowaniu mejla równiż
swoim własnym kluczem publicznym. Jak to zrobić w konfiguracji mutta?

Linie:
    set pgp_encrypt_only_command="pgpewrap gpg --batch --quiet --no-verbose --output - --encrypt --textmode --armor --always-trust -- -r %r -- %f"
    set pgp_encrypt_sign_command="pgpewrap gpg %?p?--passphrase-fd 0? --batch --quiet --no-verbose --textmode --output - --encrypt --sign %?a?-u %a? --armor --always-trust -- -r %r -- %f"
zamieniamy na (dodajemy `--encrypt-to $GPGKEY` przed `-- -r %r`):
    set pgp_encrypt_only_command="pgpewrap gpg --batch --quiet --no-verbose --output - --encrypt --textmode --armor --trust-model always --encrypt-to $GPGKEY -- -r %r -- %f"
    set pgp_encrypt_sign_command="pgpewrap gpg %?p?--passphrase-fd 0? --batch --quiet --no-verbose --textmode --output - --encrypt --sign %?a?-u %a? --armor --trust-model always --encrypt-to $GPGKEY -- -r %r -- %f"

Zmieniłem również opcję `--always-trust` (jest deprecated w mojej wersji GnuPG) na `--trust-model always`.

Użyłem zmiennej środowiskowej `$GPGKEY`. Dlaczego? Z mutta korzystam zarówno w domu, jak i w pracy, natomiast używam
różnych kluczy do szyfrowania. Dzięki temu w `.bashrc` mam coś takiego `export GPGKEY=BADC0FFEE` (na każdym
komputerze jest to inna wartość), ale za to konfigurację dla mutta jest identyczna.

Trochę musiałem poświęcić czasu go googlowanie, czytanie grup dyskusyjnych, przedzieranie się przez manuala GnuPG. A
dziś chciałem znaleźć źródlo mojej konfiguracji GnuPG dla mutta i wszedłem na [oficjalną stronę z dokumentacją](http://dev.mutt.org/trac/wiki/MuttGuide/UseGPG).
I oczywiście okazało się, że tam wszystko jest wyjaśnione. Mam nauczkę, żeby konfiguracji różnych programów szukać na
oficjalnych stronach, a nie przeglądać różne dotfiles'y na githubie. Duh.
