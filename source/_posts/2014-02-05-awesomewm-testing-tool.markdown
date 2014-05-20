---
layout: post
title: "AwesomeWM testing tool"
date: 2014-02-05 22:43
comments: true
categories: Linux
---

Najlepszym Window Managerem, jakiego kiedy używałem jest [awesome](http://awesome.naquadah.org/). Nazwa jest naprawdę adekwatna i świetnie opisuje uczucia podczas korzystania z awesome'a.

Awesome jest naprawdę super -- i to z wielu powodów. Jednym z nich jest konfiguracja w pliku tekstowym. Żadnego klikania, żadnych wizardów, konfiguratorów i innego szitu dla sofciarzy. Do tego plik konfiguracyjny to po prostu kod w lua -- można tam robić prawdziwe cuda. Fajniej się tego używa niż statycznego pliku konfiguracyjnego.

<!--more-->

Jest tylko jeden problem... gdy popsujemy coś w konfiguracji to przekonamy się o tym dopiero po restarcie window managera. Awesome w takim przypadku ładuje defaultowy config, ale jest to bardzo uciążliwe, chociażby z tego powodu, że tracimy nasze skróty klawiszowe i musimy używać domyślnym. Dla mnie wkurzające, bo kilka najczęściej używanych mam przebindowane na wygodniejsze dla mnie.

Oczywiście możemy użyć: `awesome -k` do sprawdzenia poprawności składniowej pliku konfiguracyjnego, ale wiadomo, że poprawna składnie to dopiero połowa sukcesu (albo i mniej).

Z pomocą przychodzi nam tytułowy [AwesomeWM Testing Tool](https://github.com/mikar/awmtt). Jest to prosty wrapper na [Xephyr'a](http://www.freedesktop.org/wiki/Software/Xephyr/). Xephyr jest natomiast aplikacją, która pozwala odpalić zagnieżdżony proces serwera X'ów, dzięki czemu możemy mieć w okienku odpaloną kolejną instancję awesome'a. Nie musimy restartować tej, z której obecnie korzystamy -- pozwala to na bezbolesne testowanie zmian w rc.lua.

Awmtt domyślnie wczytuje plik `~/.config/awesome/rc.lua.test`, ale ja zazwyczaj nie bawię się w tworzenie testowych plików. Przed popsuciem chroni mnie repozytorium git'a, w których trzymam moje pliki konfiguracyjne. Awmtt odpalamy tak: `awmtt start -N`. Opcja `-N` powoduje, że zostanie wczytany plik `rc.lua`, zamiast `rc.lua.test`. Zabić sesję Xephyra z działającą wewnątrz sesją X'ów wykonujemy polecenie `awmtt stop`.

No i to w zasadzie tyle, wielkiej filozofii tu nie ma. Jeszcze tylko screen pokazujący awmtt w działaniu.

[{% imgcap /images/post_images/awmtt.jpg %}](/images/post_images/awmtt.jpg)
