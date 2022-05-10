---
title: Il problema del resto
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

Immaginiamo di dover scrivere l'algoritmo di restituzione del resto per una macchinetta del caffé. Un caffé costa 70 centesimi e noi inseriamo una moneta da un euro.
Dobbiamo restituire il resto utilizzando monete da 5, 10, 20 o 50 centesimi. Come calcoliamo quali e quante monete restituire?

La logiva Greedy inizia dalla moneta più grande e poi lavora andando verso le monete più piccole.

L'algoritmo prende in esame dalla moneta più grande, quella da 50 centesimi e la confronta con il resto da dare (30 centesimi). Dato che la moneta supera il resto da dare la scarta.

L'algoritmo passa quindi alla moneta da 20 centesimi e la confronta con il resto da dare (30 centesimi). Dato che il resto da dare supera il valore della moneta decide di dare la moneta e sottrae al resto da dare il valore della moneta. (30 - 20 = 10 centesimi)

Restano da dare 10 centesimi.

L'algoritmo confronta di nuovo la moneta da 20 centesimi con il resto da dare e dato questa supera il valore del resto (20 > 10) decide che non bisogna dare altre monete da 20 centesimi.

L'algoritmo passa quindi alla moneta da 10 centesimi e la confronta con il resto da dare (10 centesimi). Dato che il resto da dare è uguale al valore della moneta decide di dare la moneta e sottrae al resto da dare il valore della moneta. (10 - 10 = 0 centesimi)

L'algoritmo decide dunque di dare una moneta da 20 centesimi ed una da 10.

L'algoritmo sembra funzionare bene, purtroppo però non viviamo in un mondo ideale in cui la macchinetta dispone sempre di tutte le monete che occorrono per dare il resto.


## Il problema del resto un po' più complicato

Una vera macchinetta del caffé dispone di un numero limitato di monete e quando deve calcolare il resto deve tenere conto delle proprie disponibilità.

Facciamo finta di disporre delle seguenti monete.

* 10 x 1 cent
* 3 x 2 cent
* 1 x 5 cent
* 0 x 10 cent 
* 1 x 20 cent 
* 19 x 50 cent

La logica Greedy, come al solito, inizia dalla moneta più grande e poi lavora andando verso le monete più piccole. In questo caso però prima di erogare la moneta controlla la disponibltà della moneta stessa.

L'algoritmo prende in esame dalla moneta più grande, quella da 50 centesimi e la confronta con il resto da dare (30 centesimi). Dato che la moneta supera il resto da dare la scarta.

L'algoritmo passa quindi alla moneta da 20 centesimi. Innanzi tutto controlla se la moneta è disponibile, quindi la confronta con il resto da dare (30 centesimi). Dato che il resto da dare supera il valore della moneta decide di dare la moneta e sottrae al resto da dare il valore della moneta. (30 - 20 = 10 centesimi). Inoltre l'algoritmo diminiusce di 1 la quantità di monete disponibili da 20 centesimi.

* 10 x 1 cent
* 3 x 2 cent
* 1 x 5 cent
* 0 x 10 cent 
* 0 x 20 cent
* 19 x 50 cent

Restano da dare 10 centesimi di resto da restiture. 

L'algoritmo controlla se c'è almeno una moneta da 20 centesimi e dato che non ce ne sono passa a valutare quella da 10.

L'algoritmo controlla se c'è almeno una moneta da 10 centesimi e dato che non ce ne sono passa a valutare quella da 5. 

L'algoritmo controlla se c'è almeno una moneta da 5 centesimi. Dato che una moneta c'è, confronta il valore della moneta con il resto da dare. Dato che 5 <= 10 decide che si può erogare la moneta. Dunque eroga la moneta e decrementa di 5 centesimi il resto da dare.

* 10 x 1 cent
* 3 x 2 cent
* 0 x 5 cent
* 0 x 10 cent 
* 0 x 20 cent
* 19 x 50 cent

Restano da dare 5 centesimi di resto. 

L'algoritmo controlla se c'è almeno una moneta da 5 centesimi e dato che non ce ne sono passa a valutare quella da 2.

L'algoritmo controlla se c'è almeno una moneta da 2 centesimi. Dato che una moneta c'è, confronta il valore della moneta con il resto da dare. Dato che 2 <= 5 decide che si può erogare la moneta. Dunque eroga la moneta e decrementa di 2 centesimi il resto da dare.

* 10 x 1 cent
* 2 x 2 cent
* 0 x 5 cent
* 0 x 10 cent 
* 0 x 20 cent
* 19 x 50 cent

Restano da dare 3 centesimi di resto. 

L'algoritmo controlla se c'è almeno una moneta da 2 centesimi. Dato che una moneta c'è, confronta il valore della moneta con il resto da dare. Dato che 2 <= 3 decide che si può erogare la moneta. Dunque eroga la moneta e decrementa di 2 centesimi il resto da dare.

* 10 x 1 cent
* 1 x 2 cent
* 0 x 5 cent
* 0 x 10 cent 
* 0 x 20 cent
* 19 x 50 cent

Resta da dare 1 centesimo di resto.

L'algoritmo controlla se c'è almeno una moneta da 1 centesimo. Dato che una moneta c'è, confronta il valore della moneta con il resto da dare. Dato che 1 <= 1 decide che si può erogare la moneta. Dunque eroga la moneta e decrementa di 1 centesim0 il resto da dare.

* 9 x 1 cent
* 1 x 2 cent
* 0 x 5 cent
* 0 x 10 cent 
* 0 x 20 cent
* 19 x 50 cent

Restano da dare 0 centesimi di resto.

L'agoritmo ha concluso erogando 1 moneta da 20 centesimi, 1 moneta da 5, 2 monete da 2 e una moneta da 1 centesimo.
