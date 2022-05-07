---
id: 124
title: 'Le liste'
date: '2020-02-04T15:21:56+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=124'
---

<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="326" loading="lazy" src="https://www.youtube.com/embed/Ustzf00v2IY?feature=oembed" title="Le liste in Python, prima parte" width="580"></iframe></div></figure>Python fornisce un tipo built-in (che fa quindi parte delle caratteristiche di base del linguaggio) chiamato lista. Questa viene solitamente usata per rappresentare una sequenza mutabile di variabili, in genere omogenei.

Come per le variabili a ciascuna lista viene assegnato un nome, in modo da potersi riferire a questa durante la scrittura del programma. È possibile creare una lista vuota usando le parentesi quadre. In questo modo si viene a creare una struttura dati che non contiene elementi al suo interno ma è pronta a riceverne.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">vuota = [] # nuova lista vuota di nome: vuota
```

</div>.

Se ho bisogno di creare una lista cui intendo inserire dei valori al momento stesso della sua creazione è sufficiente che io elenchi tra parentesi quadre questi valori separati da virgole.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">numeri = [0, 1, 2, 3] # lista di 4 elementi di tipo int 
lettere = ['a', 'b', 'c', 'd', 'e'] # lista di 5 elementi di tipo char 
parole = ['mattina', 'pomeriggio'] # lista di 2 elementi di tipo str
```

</div>Gli esempi in alto creano tre liste: la prima si chiama numeri e contiene i quattro numeri: 0, 1, 2, 3; la seconda si chiama lettere e contiene i caratteri: ’a’, ’b’, ’c’, ’d’ ed ’e’; la terza si chiama parole e contiene le stringhe di testo: ’mattina’ e ’pomeriggio’.

#### Cosa posso fare con una lista? 

Una volta immagazzinate le informazioni in una lista come le posso uti- lizzare? Le tecniche sono molteplici. Ogni lista numera internamente i suoi elementi cominciando a contare dallo 0. Si fa riferimento all’indice usando la notazione: **nomelista\[#numero indice\]**. Gli indici dunque vanno da 0 a n − 1 dove n sono gli elementi della lista. Se uso indici negativi faccio riferimento agli elementi della lista iniziando dall’ultimo e andando a ritrorso.

<figure class="wp-block-table">| indice | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| contenuto | ’a’ | ’b’ | ’c’ | ’d’ | ’e’ |
| indice | -5 | -4 | -3 | -2 | -1 |

</figure>Dunque sono sintatticamente corrette le seguenti assegnazioni:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">prima_lettera = lettere[0] # la variabile prima_lettera viene ad assumere il valore 'a' 
seconda_lettera = lettere[1] # la variabile seconda_lettera viene ad assumere il valore 'b' 
ultima_lettera = lettere[-1] # la variabile ultima_lettera viene ad assumere il valore 'e'
```

</div>Le cose funzionano anche al contrario per esempio se eseguo le istuzioni:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">lettere[0] = 'u' 
lettere[-2] = 'k'
```

</div>.

Mi ritroverò con i contenuti della lista *lettere* variati nel seguente modo.

<figure class="wp-block-table">| indice | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| contenuto | ’u’ | ’b’ | ’c’ | ’k’ | ’e’ |
| indice | -5 | -4 | -3 | -2 | -1 |

</figure>#### Operatori

Gli operatori matematici + e \* possono essere utilizzati sulle liste, vediamo qual’è il significato:

- + concatenazione: crea una nuova lista formata dall’unione degli elementi che sono all’interno della prima lista con gli elementi che sono all’interno della seconda lista
- \* ripetizione: ripete gli elementi della lista tante volte quanto indi- cato

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># operatori:
vocali = ['a', 'e'] + ['i', 'o', 'u'] 
# risultato ['a', 'e', 'i', 'o', 'u'] 
ripetute = ['a', 'e'] * 3 
# risultato ['a', 'e', 'a', 'e', 'a', 'e']
```

</div>Cosa fa secondo te il seguente programma python?

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">classe = ['Gino', 'Sandra']
nuovo_alunno = input('Digita il tuo nome: ') 
classe = classe + [nuovo_alunno]
```

</div>Il programma nalla prima istruzione crea una lista di nome classe che contiene gli elemementi ’Gino’ e ’Sandra’. La seconda istruzione consiste nel- la lettura del nome di un nuovo alunno. La terza istruzione è una istruzione complessa:

- \[nuovo\_alunno\]: prende la variabile nuovo alunno e inizializza una nuova lista che contiene unicamente questa variabile
- classe + \[nuovo\_alunno\]: concatena la lista classe con la lista appena creata al punto precedente
- assegna a classe il risultato della concatenazione della lista classe con- catenata con la lista creata al primo punto In questo modo ho aggiungere un elemento alla mia lista classe.

#### Slicing, facciamo le liste “a fette”

Attraverso l’operatore **:** si può prendere una fetta di una lista, in inglese si usa la parola slicing. La sintassi è la seguente:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># slicing:
lettere = ['a', 'b', 'c', 'd', 'e'] 
lettere_al_centro = lettere[1:4] # ['b', 'c', 'd']
```

</div>L’istruzione lettere\[1:4\] estrae dalla lista una sua sottosezione composta dagli elementi di indice 1, 2, 3. Nota bene: l’elemento di indice 4 viene escluso. Il comando dunque nomelista\[n, m\] estrae da una lista una sottolista che va dall’elemento di indice n all’elemento di indice m-1.

#### In e not in

Ci sono operatori che mi permettono di controllare se un elemento è contenuto in una lista oppure no. Sono gli operatori **in** e **not in**. Chiariamoci le idee con un esempio:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">lettere = ['a', 'b', 'c', 'd', 'e'] 
# Operatori di contenimento in e not in
if 'c' in lettere:
    # operazioni da effettuare se il carattere 'a' \`e contenuto
    print('Il carattere a e\' contenuto nella lista chiamata lettere')
if 'k' not in lettere:
    # operazioni da effettuare se il carattere 'k' non \`e contenuto
    print('Il carattere k NON e\' contenuto nella lista chiamata lettere')
```

</div>Nel primo esempio viene controllato se il carattere *‘c’* appare nella lista *lettere*. Il controllo restituisce True dato che *‘c’* appare all’interno della lista. Nel secondo esempio viene controllato se il carattere *‘k’* non è parte della lista lettere. Anche in questo caso il controllo restituisce True dato che *‘k’* non appare all’interno della lista.

Le liste supportano anche altre funzioni:

<figure class="wp-block-table">| Funzione | Cosa fa | Esempio | Risultato |
|---|---|---|---|
| **len** | Calcola la lunghezza della lista | len(lettere) | 5 |
| **min** | Trova l’elemento più piccolo | min(lettere) | ‘a’ |
| **max** | Trova l’elemento più grande | max(lettere) | ‘e’ |

</figure>#### Visitiamo la nostra lista 

<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="326" loading="lazy" src="https://www.youtube.com/embed/UR4HOGpNwY8?feature=oembed" title="Le liste in Python, seconda parte" width="580"></iframe></div></figure>Visitare o percorrere la lista, significa **prendere ad uno ad uno ciascuno degli elementi che la compongono e applicare dei comandi a ciascuno di questi**. Volendo ad esempio stampare gli elementi che compongono una lista posso scrivere il seguente codice:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">lettere = ['a', 'b', 'c', 'd', 'e']
for x in lettere:
    print(x)
```

</div>Ad ogni iterazione x assumerà il valore di uno dei caratteri contenuto nella lista lettere e lo scriverà sullo schermo.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">numeri = [1, 2, 3, 4, 5, 6] 
for x in numeri:
    if x % 2 == 0:
        print(x)
```

</div>In questo secondo esempio **percorro** la lista numeri prendendo un numero per volta ed assegnandolo ad x, quindi applica l’operatore modulo (resto della divisione intera) per vedere se il numero selezionato é pari e se ciò risulta vero lo scrivo.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># stampa il quadrato di ogni numero di seq
seq = [1, 2, 3, 4, 5]
for n in seq:
    print('Il quadrato di ', n, ' e\' ', n**2)
```

</div>E’ possibile visitare la lista anche utilizzando un ciclo **while**

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">numeri = [1, 2, 3, 4, 5, 6]
indice = 0
while indice < len(numeri):
    print(numeri[indice])
    indice = indice + 1
```

</div>## Esercizi sulle liste

**Esercizio 1:** Scrivi un programma python in cui venga inizializzata una lista di nomi contente sei nomi a piacere e che scriva i nomi contenuti nella lista iniziale utilizzando un ciclo for.

**Esercizio 2:** Scrivi un programma python in cui venga inizializzata una lista di nomi contente sei nomi a piacere e che utilizzando una stringa da utilizzarsi come accumulatore crei una stringa contenete tutti i nomi contenuti nella lista iniziale concatenati.

**Esercizio 3:** Scrivi un programma python in cui venga inizializzata una lista di nomi contente sei nomi a piacere e che utilizzando una stringa da utilizzarsi come accumulatore crei una stringa contenete tutti i nomi contenuti nella lista iniziale concatenati in questo modo: il primo nome compare una volta, il secondo nome due volte, il terzo nome tre volte ecc…

**Esercizio 4:** Scrivi un programma python in cui venga inizializzata una lista di nomi contente sei nomi a piacere e che utilizzando una stringa da utilizzarsi come accumulatore crei una lista contenete tutti i nomi contenuti nella lista iniziale in questo modo: il primo nome compare una volta, il secondo nome due volte, il terzo nome tre volte ecc…

**Esercizio 5:** Scrivi un programma python in cui venga inizializzata una lista di nomi contente sei nomi a piacere e che utilizzando una stringa da utilizzarsi come accumulatore crei una lista contenete tutti i nomi contenuti nella lista iniziale in questo modo: i nomi in posizione ad indice pari compaiono una volta, quelli ad indice dispari compationo due volte.

**Esercizio 6:** Scrivi un programma python in cui venga inizializzata una lista di nomi contente sei nomi a piacere e che utilizzando una stringa da utilizzarsi come accumulatore crei una lista contenete tutti i nomi contenuti nella lista iniziale che iniziano per vocale.

**Esercizio 7:** Inizializza una lista comprendende numeri interi a caso a tua scelta, quindi scrivi un programma Python che scandisca gli elementi della lista creata e scriva a video i soli numeri dispari

**Esercizio 8:** Inizializza due liste di numeri interi a caso a tua scelta quindi scrivi una piccola funzione che confronti le due liste e ritorni vero se hanno almeno un elemento in comune

**Esercizio 9:** Scrivi un programma Python che trovi il penultimo elemento piu’ piccolo in una lista

**Esercizio 10:** Scrivi un programma Python che calcoli la frequenza di elementi di una lista (conta il ripetersi)

**Esercizio 11:** Scrivi un programma Python che crei una lista concatenando una lista con un range di numeri es:  
 Esempio:  
 Lista iniziale : \[‘p’, ‘q’\]  
 n =5  
 Lista in Output : \[‘p1’, ‘q1’, ‘p2’, ‘q2’, ‘p3’, ‘q3’, ‘p4’, ‘q4’, ‘p5’, ‘q5’\]

**Esercizio 12:** Scrivi un programma Python che inizializzate due liste con 10 elementi a piacere trovi gli elementi comuni tra due liste e per ciascuno di questo scriva: l’elemento x è comune.

**Esercizio 13:** Scrivi un programma Python che converta una lista di multipli interi in un singolo intero  
 Esempio:  
 Lista iniziale: \[11, 33, 50\]  
 Intero in Output: 113350

**Esercizio 14:** Scrivi un programma Python che inizializzata una lista di numeri interi calcoli il prodotto di tutti i numeri contenuti nella lista

**Esercizio 15:** Scrivi un programma Python che legga 10 numeri (utilizzare un ciclo), li memorizzi in una lista e li scriva in ordine inverso.

**Esercizio 16:** Scrivi un programma Python che legga 10 numeri e memorizzi in due liste separate che si chiamano: numeri\_grandi e numeri\_piccoli. Nella prima lista vanno i numeri da più piccoli di 100, nella seconda quelli più grandi. I numeri negativi non vanno memorizzati.