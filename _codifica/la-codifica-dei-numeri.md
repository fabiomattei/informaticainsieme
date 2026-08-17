---
id: 558
title: La codifica dei numeri
date: '2020-10-07T00:08:33+02:00'
author: Fabio Mattei
layout: page
---

![Il numero binario 1011 convertito in decimale con le potenze di 2](/images/codifica/la-codifica-dei-numeri/la-codifica-dei-numeri.svg){:class="aside-image"}

Il computer, come abbiamo visto, riesce a rappresentare qualsiasi informazione utilizzando soltanto due simboli: **0** e **1**.

Anche i numeri, quindi, devono essere codificati usando esclusivamente questi due simboli. Il sistema che permette di farlo si chiama **sistema binario**.

### Il sistema decimale che conosciamo

Noi siamo abituati a contare usando il **sistema decimale**, che utilizza dieci simboli: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.

Quando scriviamo un numero come **101**, in realtà attribuiamo un significato a ciascuna cifra in base alla posizione che occupa: partendo da destra il **sistema decimale** o **base 10**.

Quando scriviamo un numero come **101**:

| posizione | 10³ | 10² | 10¹ | 10⁰ |
|---|---|---|---|
| valore | 1000 | 100 | 10 | 1 |
| cifra | 0 | 1 | 0 | 1 |

Il valore complessivo è 0×1000 + 1×100 + 0×10 + 1×1 = 101.

Ogni cifra vale una **potenza di 10** in base alla sua posizione: si moltiplica la potenza di  10 corrispondente al valore della cifra, poi si sommano i risultati.

Lo stesso principio si applica al **sistema binario**, che utilizza gli stessi principi delle **potenze**, ma con base **2** invece di base 10: ogni posizione della cifra vale una **potenza di 2** invece che una potenza di 10.

### Esempio: dal binario al decimale

Prendiamo il numero binario **1011**:

| posizione | 2³ | 2² | 2¹ | 2⁰ |
|---|---|---|---|---|
| valore | 8 | 4 | 2 | 1 |
| cifra | 1 | 0 | 1 | 1 |

Il valore decimale si ottiene sommando le potenze di 2 corrispondenti alle cifre pari a 1.

8 + 0 + 2 + 1 = **11**

Quindi il numero binario 1011 corrisponde al numero decimale 11.

### Quanti numeri si possono rappresentare?

Con **n bit** a disposizione possiamo rappresentare **2ⁿ numeri diversi**, da 0 a 2ⁿ-1.

Ad esempio con 8 bit (un **byte**) possiamo rappresentare 2⁸ = 256 numeri diversi, da 0 a 255.

| bit disponibili | numeri rappresentabili |
|---|---|
| 1 | 2 (0 e 1) |
| 4 | 16 (da 0 a 15) |
| 8 | 256 (da 0 a 255) |
| 16 | 65.536 |

### Perché usare il binario?

Il computer è costituito da circuiti elettronici che possono trovarsi soltanto in due stati: **acceso** o **spento**, corrente che passa o che non passa. Questi due stati si prestano perfettamente ad essere rappresentati dai due simboli 0 e 1.

Ecco perché tutta l'informazione gestita da un computer, numeri compresi, viene tradotta in sequenze di bit.
