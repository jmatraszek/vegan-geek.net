---
layout: post
title: "Restore MySQL dumpa z progress barem"
date: 2013-10-02 19:39
comments: true
categories: Linux
---
W pracy często muszę robić dumpy baz danych. A jak robię dumpy to potem je muszę restore'ować. Restore jak to restore --
przy większych bazach potrafi chwilę trwać. Ale to dobrze -- kod w Ruby nie jest kompilowany, więc robienie restore'a
jest niezłą wymówką, żeby móc toczyć [walki na miecze](http://xkcd.com/303/). Tylko czasami przydałoby się wiedzieć ile
takie wgrywanie dumpa może trwać...

<!--more-->

Tu z pomocą przychodzi nam program [pv](http://linux.die.net/man/1/pv).

Zwykłego dumpa wgrywamy tak: `mysql -u USER -h HOST -p DATABASE < dump.sql`

Dumpa z progress barem (z wykorzystaniem pv) robimy tak: `pv --progress --timer --eta --bytes --rate dump.sql | mysql -u USER -h HOST -p DATABASE`

Efekt bardzo dobry, tylko trochę żal tyle znaków pisać. A jako, że dumpy wgrywam bardzo często to postanowiłem napisać
sobie małego wrappera w BASHu (sic!).

Wymagania? Ma się odpalać w sposób jak najbardziej zbliżony do zwykłego restore'a. Ma supportować różne opcje mysqla
(wszystkie, czasami robię z różnymi -- z hasłem, bez hasła, bez hosta etc.).

Wyszło mi coś takiego:
    #!/bin/sh
    pv --progress --timer --eta --bytes --rate | mysql "$@"

Cały trik polega na tym, że pełną listę argumentów wywołania przekazuję do mysqla. Plik z dumpem nie jest podawany jako
argument, tylko przekierowywany na standardowe wejście skryptu. pv wywołujemy bez nazwy pliku, więc będzie czytał właśnie
ze standardowego wejścia. Dzięki temu składnia wywołania do złudzenia przypomina zwykłego restore'a robionego przy użyciu
mysql.

Plik zapisujemy pod jakąś wygodną nazwą (ja sobie zapisałem jako `mypvsql`), umieszczamy gdzieś w `$PATH`, nadajemy prawa do
wykonywania (`chmod +x /path/to/mypvsql`) i używamy: `mypvsql -u USER -p DATABASE < dump.sql`.

Bierzcie i restore'ujcie z tego wszyscy.
