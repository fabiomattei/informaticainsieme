---
title: Greedy
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

Gli algoritmi di tipo Greedy sono una classe di algoritmi che hanno una logica di fondo comune. Gli algoritmo di tipo Greedy mirano a trovare una soluzione ottima globale cercando di fare la scelta migliore ad ogni passo.
Quando si presenta una scelta l'algoritmo sceglie la strada che permette di avere il massimo vantaggio senza cercare di intepretare il futuro e senza rimettere in discussione
le scelte fatte in passato.

Sono importanti perché anche se non portano necessariamente ad una soluzione ottima portano a trovare una soluzione buona facendo un efficiente uso del tempo.

Si riconosce un algoritmo Greed da questo tipo di ragionamento:

__In questo momento esatto qual'è la scelta migliore da fare?__

Gli algoritmi Greedy sono voraci, a loro non interessa la visione globale, vogliono ottenere il massimo vantaggio ad ogni passo, sono interessati solo dalla soluzione ottima locale. Questo significa che una soluzione ottima globale potrebbe basarsi su scelte di tipo diverso.

Gli algoritmi Greedy non si guardano mai indietro e non si mettono mai in discussione.

Gli algoritmi Greedy fanno un uso del tempo efficiente, generalmente trovano una buona soluzione in un tempo accettabile.
Qualche volta accade che l'algoritmo Greedy trovi la soluzione ottima.


## Il problema del resto

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



## Il problema di Knapsack



## Algoritmo di Dijkstra

L'algoritmo di Dijkstra trova il cammino più breve che collega un nodo radice ad ogni altro nodo del grafo. Nel nostro esempio utilizzereemo un grafo orientato pesato, ogni arco ha una direzione ed un peso.

L'agoritmo di Dijkstra ha molti utilizzi. Può essere utilizzato per calcolare il percorso su di una mappa. Inoltre è utilizzato per:

* IP Routing
* A* Algorithm
* Telephone networks

L'algoritmo segue queste regole:

* Ogni volta che vogliamo visitare un nuovo nodo si sceglie quello collegato alla minima distanza tra i nodi da visitare.
* Una volta scelto il nodo controlliamo tutti i nodi suoi vicini e calcoliamo la distanza tra i vicini e la radice sommando i pesi dai percorsi che includono il nuovo nodo selezionato.
* Se la distanza appena calcolata è minore della distanza conosciuta in precedenza, aggiorneremo la distanza più corta.

Per lavorare con l'algoritmo si crea una tabella che indica la distanza complessiva tra la radice e ciascun nodo e si pone il suo valore ad infinito per ciascun nodo.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | infinito        |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | infinito        |              |          |

Immaginiamo di trovarci di fronte il seguente grafo:

![Dijkstra, grafo iniziale](/informaticainsieme/images/algoritmi/greedy/dijkstra01.png){:class="aside-image"}


![Dijkstra: aggiungo nodo A](/informaticainsieme/images/algoritmi/greedy/dijkstra02.png){:class="aside-image"}

Iniziamo ad applicare l'algoritmo di Dijkstra a partire dal nodo A che considereremo nodo radice. Consideriamo tutti i suoi vicini e calcoliamo la distanza da A.

La tabella delle distanza aggiornata è la seguente:

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 6               | A            |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | 3               | A            |          |

Il nodo a costo minimo da raggiungere è il nodo E quindi vado a considerare il nodo E.

![Dijkstra: aggiungo nodo E](/informaticainsieme/images/algoritmi/greedy/dijkstra03.png){:class="aside-image"}

Una volta in E rivaluto le distanze dei vicini di E non ancora visitati. 
Noto che D, è raggiungibile a costo 7 da E, quindi è raggiungibile a costo complessivo 10 da A.
Noto che C, è raggiungibile a costo 4 da E, quindi è raggiungibile a costo complessivo 7 da A.
Dato che B è raggiungiibile da C rivaluto il costo di B. Il costo inizale era 6, ma siccome B dista 2 da E posso raggiungerlo a costo 5 passando per E.

Ecco la tabella aggiornata.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            |          |
| C    | 4               | E            |          |
| D    | 10              | E            |          |
| E    | 3               | A            | Si       |

Per andare al passo successivo guardo tutti i nodi rimasti e scelgo quello a costo minore quindi vado a valutare C che ha costo 4.

![Dijkstra: aggiungo nodo C](/informaticainsieme/images/algoritmi/greedy/dijkstra04.png){:class="aside-image"}

Una volta in C rivaluto le distanze dei vicini di C non ancora visitati.
Noto che da C è raggiungibile D a costo 3. Dato che arrivare a C ha costo 4, il costo complessivo sarebbe 7. Al momento il costo per arrivare a D passando per E è 10 quindi il percorso che passa per C è conveniente.
Da C è raggiungibile B a costo 6. Dato che arrivare a C ha costo 4, il costo complessivo per arrivare a B passando da C sarebbe 10. Tuttavia è già possibile arrivare a B con un costo minore, quindi scarto questa possibilità.


| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            |          |
| C    | 4               | E            | Si       |
| D    | 7               | C            |          |
| E    | 3               | A            | Si       |

Per andare al passo successivo guardo tutti i nodi rimasti e scelgo quello a costo minore. Sono rimasti B e D e il costo minore è rappresentato da B.

![Dijkstra: aggiungo nodo B](/informaticainsieme/images/algoritmi/greedy/dijkstra05.png){:class="aside-image"}

Una volta in B rivaluto le distanze dei suoi vicini non visitati.
L'unico rimasto è il nodo D che è distante 4 da B. Dato che arrivare a B mi è costato 5 il costo complessivo per arrivare a D passando per B sarebbe 9. Tuttavia il costo attuale di B è minore di 9 quindi non cambio nulla nella tabella.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            | Si       |
| C    | 4               | E            | Si       |
| D    | 7               | C            |          |
| E    | 3               | A            | Si       |

![Dijkstra: aggiungo nodo D](/informaticainsieme/images/algoritmi/greedy/dijkstra06.png){:class="aside-image"}

A questo punto rimane solo il nodo D e lo aggiungo senza il bisogno di rivalutare nulla.

Ecco dunque la tabella indicante le distanze minime a partire dal nodo A.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            | Si       |
| C    | 4               | E            | Si       |
| D    | 7               | C            | Si       |
| E    | 3               | A            | Si       |

L'algoritmo di Dijkstra è stato completato.



## Algoritmo di Prim

E' un algoritmo molto simile a quello di Dijkstra, la differenza nella meccanica è che al posto del costo complessivo si tiene conto del costo passo per passo. 

L'algoritmo di Prim serve a calcolare l'albero coprente minimo per un grafo pesato non orientato.

L'algoritmo segue queste regole:

* Ogni volta che vogliamo visitare un nuovo nodo si sceglie quello collegato alla minima distanza tra i nodi da visitare.
* Una volta scelto il nodo controlliamo tutti i nodi suoi vicini e la confrontiamo che le distanze minime che abbiamo ottenuto fino ad ora per arrivare a quei nodi.
* Se la distanza appena calcolata per arrivare ad un nodo è minore della distanza conosciuta in precedenza, aggiorneremo la distanza più corta.

Riprendiamo in esame il grafo precedente e immaginiamo di partire sempre dal nodo A.


| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | infinito        |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | infinito        |              |          |

![Prim, grafo iniziale](prim01.png)

Consideriamo tutti i suoi vicini e calcoliamo la distanza da A.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 6               |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | 3               |              |          |

Scegliamo il ramo con distanza minore e passiamo al passo successivo. Dato che il ramo pù corto è quello che collega E.

![Prim, grafo iniziale](prim02.png)

A questo punto 



## Algoritmo di Kruskal
