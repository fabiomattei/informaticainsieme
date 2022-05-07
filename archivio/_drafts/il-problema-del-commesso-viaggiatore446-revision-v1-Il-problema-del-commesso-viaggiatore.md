---
id: 601
title: 'Il problema del commesso viaggiatore'
date: '2021-03-06T09:40:27+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2021/03/06/446-revision-v1/'
permalink: '/?p=601'
---

*Un commesso viaggiatore deve visitare i suoi clienti. Conosce la posizione dei clienti e la distanza che li separa. Vuole visitare tutti i clienti nell’ordine che gli consente di percorrere la strada più corta possibile. Come fa?*

In termini più formali, il problema consiste nel costruire un grafo i cui nodi rappresentano i clienti e la casa del commesso, mentre gli archi rappresentano i percorsi fra i nodi, e di trovare su di esso un ciclo che tocchi tutti i nodi e abbia la distanza complessiva minima.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Example_The_travelling_salesman_problem_TSP.gif)</figure><figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="326" loading="lazy" src="https://www.youtube.com/embed/9tH9oDTQlCA?feature=oembed" title="Problem solving: il problema del commesso viaggiatore" width="580"></iframe></div></figure>Il problema, semplice da descrivere, è però complesso da risolvere. Il numero delle sue soluzioni, infatti, cresce molto rapidamente con il numero dei nodi.

Il TSP (travelling salesman problem) fu formulato per la prima volta dal matematico irlandese [Sir William Rowan Hamilton](http://www-groups.dcs.st-andrews.ac.uk/~history/Mathematicians/Hamilton.html). Nel 1857, a Dublino, egli descrisse un gioco, detto *Icosian game*, a una riunione della British Association for the Advancement of Science. Il gioco consisteva nel trovare un percorso che toccasse tutti i vertici di un *icosaedro*, passando lungo gli spigoli, ma senza mai percorrere due volte lo stesso spigolo. Come si può vedere nella figura seguente, l’icosaedro ha 12 vertici, 30 spigoli e 20 facce identiche a forma di triangolo equilatero.

Il gioco, venduto alla ditta J. Jacques and Sons per 25 sterline, fu brevettato a Londra nel 1859, ma vendette pochissimo.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/icosian_game.png)</figure>Al fine di risolvere i problemi ci occorrono alcune definizioni:

- Due nodi si dicono *adiacenti* se sono collegati da un arco;
- Il numero di archi che “escono” da un nodo si dice *valenza* del nodo;
- Un *percorso* (o *cammino*) tra due nodi del grafo consiste in una sequenza di nodi ciascuno dei quali (tranne l’ultimo) è adiacente con il successivo; un percorso può, quindi essere descritto con una lista di nodi;
- Un *ciclo* è un percorso che inizia e termina nello stesso nodo;
- Un percorso si dice *semplice* se *non* ha nodi ripetuti;

#### Esempio

È dato un grafo descritto dal seguente elenco di archi:

- arco(n1,n2,2)
- arco(n2,n3,2)
- arco(n4,n1,1)
- arco(n4,n2,4)
- arco(n3,n1,5)
- arco(n4,n5,3)

Disegnare il grafo e trovare:

1\. la lista L1 del percorso più breve tra n5 e n3 e calcolarne la lunghezza K1;  
 2. la lista L2 del percorso più lungo (senza passare più volte per uno stesso nodo) tra n5 e n3 e calcolarne la lunghezza K2.

COMMENTI ALLA SOLUZIONE  
Per disegnare il grafo si osservi innanzitutto che vengono menzionati 5 nodi (n1, n2, n3, n4, n5); si procede per tentativi: si disegnano i 5 punti nel piano e li si collega con archi costituiti da segmenti: probabilmente al primo tentativo gli archi si incrociano;

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Schermata-2020-02-21-alle-20.35.02-1024x454.png)</figure>Si noti che le lunghezze degli archi che compaiono nei termini (che rappresentano delle strade) *non* sono (necessariamente) proporzionali a quelle degli archi del grafo (che sono, segmenti di retta).  
Per rispondere alle due domande occorre elencare sistematicamente *tutti* i percorsi, che non passino più volte per uno stesso punto, tra n5 e n3:

<figure class="wp-block-table">| Percorso da n5 a n3 | Lunghezza |
|---|---|
| \[n5, n4, n1, n2, n3\] | 3+1+2+2=8 |
| \[n5, n4, n1, n3\] | 3+1+5=9 |
| \[n5, n4, n2, n3\] | 3+4+2=9 |
| \[n5, n4, n2, n1, n3\] | 3+4+2+5=14 |

</figure>#### Problema 1

Un grafo (che corrisponde alla rete di strade che collegano delle città) è descritto dal seguente elenco di archi:

- a(n1,n2,13)
- a(n2,n3,3)
- a(n3,n4,13)
- a(n1,n4,3)
- a(n4,n5,3)
- a(n5,n1,5)
- a(n2,n5,7)
- a(n3,n5,11)

Disegnare il grafo e trovare:

1. la lista L1 del percorso semplice più breve tra n1 e n3;
2. la lista L2 del percorso semplice più lungo tra n1 e n3.

#### Problema 2

Un commesso viaggiatore deve visitare un insieme di città, tornando nel punto di partenza e senza passare due volte per la stessa città (ovvero facendo un *tour*).  
 Le distanze tra le coppie di città, in chilometri, sono date dai seguenti termini (che hanno la struttura arco(&lt;nome di città&gt;,&lt;nome di città&gt;,&lt;distanza&gt;):

- arco(C1,C2,10)
- arco(C1,C3,4)
- arco(C1,C4,9)
- arco(C2,C3,6)
- arco(C2,C4,5)
- arco(C3,C4,4)

Il commesso parte dalla città C1; (disegnare il grafo delle città e) trovare la lunghezza, in chilometri del *tour* più corto.

#### Problema 3

Un grafo (che corrisponde alla rete di strade che collegano delle città) è descritto dal seguente elenco di archi:   
a(n1,n2,13) a(n2,n3,3) a(n3,n4,13) a(n1,n4,3) a(n4,n5,3) a(n5,n1,5) a(n2,n5,7) a(n3,n5,11)   
Disegnare il grafo e trovare: la lista L1 del percorso semplice più breve tra n1 e n3; la lista L2 del percorso semplice più lungo tra n1 e n3.

#### Problema 4

Un grafo, che si può immaginare come rete di strade (archi) che collegano delle città (nodi), è descritto dal seguente elenco di archi: a(n1,n3,3) a(n5,n1,4) a(n3,n5,3) a(n2,n4,5) a(n6,n4,2) a(n1,n2,8) a(n2,n5,1) a(n5,n6,3) a(n1,n6,8)   
Determinare:  
1\. la lista L1 del percorso semplice più breve tra n1 e n6 e calcolarne la lunghezza K1;   
2\. la lista L2 del percorso semplice più lungo tra n1 e n6 e calcolarne la lunghezza K2;   
3\. la lista L3 del percorso semplice più breve tra n1 e n6 che abbia almeno 3 archi e calcolarne la lunghezza K3.