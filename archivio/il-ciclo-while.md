---
id: 92
title: 'Il ciclo While'
date: '2020-02-04T08:12:47+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=92'
---

<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="326" loading="lazy" src="https://www.youtube.com/embed/x1GTNxCnJ-A?feature=oembed" title="Il ciclo while in Python" width="580"></iframe></div></figure>Il costrutto **while** in python permette di eseguire dei comandi un certo numero di volte. Si chiama ciclo perchè i comandi contenuti al suo interno vengono ripetuti **ciclicamente**.

La sintassi è la seguente:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">while <espressione booleana>:
    <comando 1>
    <comando 2>
    <comando 3>
```

</div>While è un ciclo con **controllo in testa** questo significa che *il controllo sull’espressione booleana viene fatto prima di entrare nel ciclo*.

Le istruzioni all’interno del ciclo vengono eseguite se il risultato dell’espressione booleana è **vero**. In caso contrario (espressione booleana falsa) il blocco comandi interno al ciclo viene ignorato e l’esecuzione del programma continua con la prima istruzione successiva al ciclo.

Scrivi ed esegui il seguente programma:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">cont = 0
while cont < 10:
   cont = cont + 1
   print(cont)
```

</div><figure class="wp-block-table">| cont | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|

</figure>Questo programma ci mostra un ciclo le cui istruzioni contenute all’interno vengono eseguite *fintanto che cont &lt; 10*. La **variabile cont** si dice **contatore** dato che il suo scopo è contare il numero di volte che il ciclo è stato eseguito. Questo costrutto è molto diffuso.

Facciamo un secondo esempio:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">cont = 0
while cont < 10:
   cont = cont + 1
print(cont)
```

</div>Noterai che l’istruzione print(cont) non è posta all’interno del ciclo (non è contenuta) dato che la sua indentazione è allineata alla parola while che definisce il ciclo e non più a destra di questa. Questo ciclo scriverà sulla console il solo numero 10.

Ricorda che l’indentazione in Python è molto importante!

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">cont = 0
while cont < 10:
    cont = cont + 1
    print(cont, end = ' ') 
```

</div>Cosa noti?

Scriviamo ora un programma che calcoli la somma dei primi 10 numeri interi

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">n = 10
cont = 1             # variabile contatore
acc = 0              # variabile accumulatore
while cont <= n:
    acc = acc + cont # incremento l'accumulatore di cont
    cont= cont + 1   # incremento il contatore di 1
print(acc)           # al termine del ciclo scrivo il valore di acc
```

</div>Nel precedente esempio abbiamo introdotto il concetto di **accumulatore**. Si dice accumulatore una variabile che *accumula dopo ciascuna esecuzione delle istruzioni all’interno del ciclo i risultati di un calcolo*. Nell’esempio ad acc viene sommato di volta in volta il contento della variabile contatore. Possiamo vedere come le variabili si comportano in una tabella di tracciamento:

<figure class="wp-block-table">| **n** | 10 |  |  |  |  |  |  |  |  |  |  |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **cont** | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| **acc** | 0 | 1 | 3 | 6 | 10 | 15 | 21 | 28 | 36 | 45 | 55 |

</figure><div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Questo programma calcola la sequenza di Fibonacci.
acc_a = 0
acc_b = 1
cont = 0
max_cont = 20
while cont < max_cont:
  cont = cont + 1
  # Occorre tenere traccia finché ci sono cambiamenti
  vecchio_acc_a = acc_a
  vecchio_acc_b = acc_b
  acc_a = vecchio_acc_b
  acc_b = vecchio_acc_a + vecchio_acc_b
  # Attenzione che la virgola alla fine di un istruzione print
  # prosegue la stampa sulla stessa linea.
  print(vecchio_acc_a)
print()
```

</div>L’algoritmo di Fibonacci funziona su di un gioco di due variabili accumulatori: acc\_a e acc\_b.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Attende sino a quando non viene inserita la giusta password.
# Usate Control-C per fermare il programma senza password.
# Notate che se non viene inserita la giusta password, il ciclo
# while prosegue all’infinito.
password = "foobar"
# Notate il simbolo != (diverso).
while password != "unicorn":
   password = input("Password:")
print "Benvenuto"
```

</div><figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="326" loading="lazy" src="https://www.youtube.com/embed/kAnH-nfn8D0?feature=oembed" title="Il ciclo While in Python con variabile flag" width="580"></iframe></div></figure>**Esercizio 1:**

Scrivere un programma che letto un numero intero N calcoli la somma di tutti i numeri da 1 ad N

**Esercizio 2:**

Scrivere un programma che letto un numero intero N ed un numero intero M (con N&lt;M) calcoli la somma di tutti i numeri da N ad M (estremi inclusi)

**Esercizio 3:**

Scrivere un programma che letto un numero intero N calcoli il fattoriale di N ( 1 \* 2 \* 3 \* 4 …. \* N)

**Esercizio 4:**

Scrivere un programma che letto un numero intero N ed un numero intero M (con N&lt;M) calcoli la somma di tutti i numeri pari compresi tra N ed M

**Esercizio 5:**

Scrivere un programma che letti N numeri in ingresso calcoli il numero massimo e il numero minimo inseriti

**Esercizio 6:**

Scrivere un programma che letti N numeri in ingresso calcoli la somma di tutti i numeri mostrando le somme parziali ogni 3 numeri inseriti

**Esercizio 7:**

Scrivere un programma che letti N numeri in ingresso calcoli la somma di tutti i numeri mostrando le somme parziali ogni 3 numeri inseriti

**Esercizio 8:**

Scrivere un programma che letto un numero intero N calcoli la somma di tutte le cifre dispari che lo compongono

**Esercizio 9:**

Scrivere un programma che letto un numero intero N scriva la tavola pitagorica della moltiplicazione del numero prescelto

**Esercizio 10:**

Scrivere un programma che scriva la tavola pitagorica della moltiplicazione completa

**Esercizio 11:**

Leonardo Pisano propose nel tredicesimo secolo il seguente problema:

Immaginiamo di chiudere una coppia di conigli in un recinto sapendo che per ogni coppia di conigli valgono le seguenti condizioni:

- Inizia a generare dal secondo mese di età
- Genera una nuova coppia ogni mese
- Non muore mai;

Quante coppie di conigli ci saranno dopo un anno?

E quante dopo un numero di mesi N, letto in input?