---
title: 'Flussi in una rete di canali'
date: '2020-04-18'
author: Fabio Mattei
layout: page
---


Sul fianco di una montagna esistono numerose sorgenti. L’acqua di una sorgente, che si suppone fluire in
modo continuo e costante, può scorrere a valle attraverso uno o più canali. Può avvenire che uno o più canali
convergano in un punto in cui esiste un’altra sorgente; in tal caso, la loro acqua si aggiunge a quella fornita
dalla sorgente raggiunta. Questa situazione è descrivibile con un reticolo di nodi (le sorgenti) collegati da
archi (i canali). Nel seguito per "litri erogati” si intenderà "litri di acqua erogati al minuto".

Di sorgenti ne esistono di due tipi:

* sorgente semplice descritta dalla tabella: s(nome della sorgente,litri erogati)
Da questa sorgente escono uno o più canali, dai quali uscirà la stessa quantità d’acqua
Esempio. La sorgente s(a,12) ha tre canali in uscita. Allora in ogni canale scorreranno 4 litri
d’acqua (4 l x 3 uscite =12 l totale)

* sorgente con valvola selezionatrice descritta dalla tabella: sv(nome della sorgente,litri erogati,litri d’acqua uscenti dal canale destro)
Questa sorgente ha sempre due sole uscite; la valvola permette di diversificare le quantità
d’acqua uscenti dal canale destro rispetto a quello di sinistra

Esempio. La sorgente sv(b,8,5) eroga 8 litri, di cui 5 escono dal canale di destra e 3 da quello di sinistra.

Un canale è descritto da una tabella: r(nome della sorgente a monte,nome della sorgente a valle,litri in perdita)
che specifica la presenza di un canale che porta acqua da monte a valle, ma che nel percorso ha delle perdite
dovute a usura. Nel caso abbia zero perdite non scriveremo il terzo dato.

Esempi:

- r(a,b) descrive un canale che collega la sorgente a con la b e nel percorso non ha perdite
- r(b,f,2) descrive un canale che collega le sorgenti b,f e nel percorso perde 2 litri
- r(c,f,0.4) descrive un canale che collega le sorgenti c,f e nel percorso perde 0.4 litri

**Quando l’acqua di un canale raggiunge una sorgente essa si distribuisce equamente nei canali
che partono da questa.**

Esempio 1

Dai canali a monte di una sorgente, che eroga 7 litri di acqua al minuto, giungono 8 litri.
A valle della sorgente partono tre canali. In ognuno di essi scorreranno (8+7)/3 = 5 litri al minuto.

Esempio 2

Dai canali a monte di una sorgente con valvola, che eroga 7 litri di acqua al minuto, giungono 8 litri.
Dalla sorgente escono 5 litri nel canale destro e 2 litri in quello sinistro.
Allora:

- gli 8 litri a monte si ripartiscono equamente nei due canali
- nel canale di destra scorreranno 9 litri (5 litri della sorgente + 4 litri dai canali a monte)
- nel canale di sinistra scorreranno 6 litri (2 litri della sorgente + 4 litri dai canali a monte)

Nel seguito alcuni esempi di esercizi che verranno proposti nelle gare.


ESERCIZIO 1

Un reticolo di canali è descritto dalle seguenti due tabelle:
s(a,6), s(b,5), s(c,1), s(d,4), s(e,3), s(f,2)
r(a,c), r(a,d), r(b,d), r(c,e),r(d,e), r(d,f)
Disegnare il reticolo, evitando incroci, e determinare la quantità di acqua che esce dai nodi c, e, f.

SOLUZIONE

|c |4  |
|e |13 |
|f |8  |


ESERCIZIO 2

Un reticolo di canali è descritto dalle seguenti due tabelle:
sv(a,6,2), s(b,2), s(c,3), sv(d,8,6), s(e,5), s(f,2)
r(a,b), r(a,c,0.5), r(d,c,1), r(d,e), r(b,f,1), r(c,f,0.8), r(e,f,0.3)
determinare la quantità di acqua che esce dal nodo-sorgente f.

|f |22.4  |