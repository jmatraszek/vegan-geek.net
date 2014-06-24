---
layout: post
title: "Zoom in/out jako page up/down"
date: 2014-06-24 20:28
comments: true
categories:
---

Klawiatury to jedyna rzecz, którą Microsoft robi dobrze. Od kilku lat
używam [Microsoft Comfort Curve
2000](http://www.microsoft.com/hardware/en-us/d/comfort-curve-keyboard-2000).
W pracy chciałem czegoś bardziej ergonomicznego i poprosiłem o [Microsoft
Natural Ergonomic
4000](http://www.microsoft.com/hardware/en-us/p/natural-ergonomic-keyboard-4000).
Klawiatura jest naprawdę świetna, choć trzeba się do niej nieco
przyzwyczaić. Świetny kształt, przydatne klawisze multimedialne,
zdejmowana podkładka -- plusów jest naprawdę dużo. Przeszkadzała mi tylko
jedna rzecz -- nie mogłem używać wbudowanego przycisku (dźwigni) zoom
in/out.

{% img /images/post_images/4000_zoom_inout.jpg  %}

<!--more-->

Sam zoom wydaje mi się mało użyteczny i nawet gdyby ta dźwignia działała
to chciałbym jakoś inaczej ją wykorzystać. Najlepszym rozwiązaniem było
dla mnie przemapowanie tego na page up/down. Zawsze mnie irytowało, że gdy
chcę przewijać output w terminalu to muszę prawą rękę zabrać z "home row"
i przesunąć ją nad klawisze strzałek, gdzie zazwyczaj page up i down są
umieszczone. Pierwsze co przyszło mi do głowy to wykorzystanie
``xmodmap``, ale problem w tym, że przy wykorzystaniu ``xmodmap`` nie
rozpoznaje klawiszy, których keycode jest większy niż 255. A w przypadku
zoom in/out tak właśnie jest. Wykorzystałem więc ``udev`` to osiągnięcia
tego co chciałem.

Tworzymy plik ``/etc/udev/hwdb.d/60-keyboard.hwdb`` o następującej
zawartości:
    keyboard:usb:v045Ep00DB*
      KEYBOARD_KEY_c022d=pageup
      KEYBOARD_KEY_c022e=pagedown

Następnie wydajemy z konsoli dwa polecenia:
    sudo udevadm hwdb --update
    sudo udevadm control --reload
oraz odłączamy i podłączamy klawiaturę.

Smacznego!
