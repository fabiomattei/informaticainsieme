---
id: 544
title: 'Il ciclo for'
date: '2020-05-17T22:38:43+02:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/05/17/118-revision-v1/'
permalink: '/?p=544'
---

<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper"><iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="326" loading="lazy" src="https://www.youtube.com/embed/9ZTyZnUvYqM?feature=oembed" title="Il ciclo for in python: variabile contatore e variabile accumulatore" width="580"></iframe></div></figure>Il ciclo for ci permette di iterare su tutti gli elementi di una lista ed eseguire un determinato blocco di codice.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">for variabile in lista:
    #elabora su variabile
    
    
```

</div><div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">#Esempio 1:
for a in [5,10,15,20,25]:
    print(a)
    
```

</div><div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">#Esempio 2:
seq = [1, 2, 3, 4, 5]
for n in seq:
    print('Il numero ', n, ' e\' ', end=' ')
    if n % 2 == 0:
        print('pari')
    else:
        print('dispari')
```

</div>#### **range**

Dato che spesso accade di voler lavorare su sequenze di numeri, Python fornisce una funzione chiamata **range** che permette di specificare uno valore iniziale o ***start*** (incluso), un valore finale o ***stop*** (escluso), e una lunghezza di passo ***step***, e che ritorna una sequenza di numeri interi:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Esempio di uso di range con start, stop e step

for x in range(1, 7, 1):
    print(x)
# scrive la sequenza di numeri: 1, 2, 3, 4, 5, 6
# notare che il valore 7 viene escluso
```

</div>Posso utilizzare range con due soli argomenti

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Esempio di uso di range con start e stop
# Step viene implicitamente posto a 1
for x in range(1, 5):
    print('Quadrato di ', x, ': ', x**2)
# scrive:
# Quadrato di 1: 1
# Quadrato di 2: 4
# Quadrato di 3: 9
# Quadrato di 4: 16
# Notare che 5 viene escluso ed il passo (step) vale 1
```

</div>Posso utilizzare range anche con un solo argomento

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Esempio di uso di range con stop
# Start e Step vengono implicitamente posti a 1
for x in range(7):
    print(x)
# scrive la sequenza di numeri: 1, 2, 3, 4, 5, 6
# notare che il valore 7 viene escluso e il passo vale 1
```

</div>#### Variabili accumulatori e ciclo for

Utilizzare il ciclo for con una variabile accumulatore è molto semplice. Nel seguente esempio si vede come si usa l’accumulatore per sommare 6 numeri.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Esempio di uso di range con start, stop e step
# Esempio di uso di accumulatore per la somma
acc = 0
for x in range(1, 7, 1):
    acc = acc + x

print(x) # nota che questa istruzione e' fuori dal ciclo
```

</div>Nel seguente esempio si vede come si usa l’accumulatore per moltiplicare 6 numeri.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Esempio di uso di range con start, stop e step
# Esempio di uso di accumulatore per la moltiplicazione
acc = 1
for x in range(1, 7, 1):
    acc = acc * x

print(x) # nota che questa istruzione e' fuori dal ciclo
```

</div>#### Istruzioni di lettura ripetute

Lo scopo fondamentale per cui esistono i cicli è quello di ripetere un certo numero di operazioni. Se devo per esempio leggere 10 numeri e sommarli tra loro, potrei fare così:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">acc = 0
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))
acc = acc + int(input("Numero: "))

print(acc)
```

</div>Oppure potrei fare così:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">acc = 0
for x in range(11):
    acc = acc + int(input("Numero: "))
    
print(acc)
```

</div>Notate come il ciclo for mi ha permesso di ridurre molto le istruzioni che ho scritto. Cosa sarebbe successo se avessi dovuto leggere 100 numeri?

#### Il campione

Può capitare di dover leggere una sequenza di numeri e trovare il più grande. In questo caso utilizzo una variabile **campione** che ciclo dopo ciclo conterrà il valore migliore trovato. Potrò utilizzare una condizione per confrontare ad ogni iterazione il valore letto con il campione ottenuto fino a quel momento.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">massimo = 0
for x in range(11):
    num = int(input("Numero: "))
    if num > massimo:
        massimo = num

print(massimo)
```

</div>#### Esercizi

**Esercizio 1**: Scrivere un programma che scriva tutti i numeri da 1 a 100

**Esercizio 2**: Scrivere un programma che scriva tutti i numeri pari da 1000 a 5000.

**Esercizio 3**: Scrivere un ciclo che sommi tutti i numeri dispari minori di 100. Scrivere la somma ottenuta.

**Esercizio 4**: Scrivere un ciclo che letto un numero N scriva i dieci numeri pari successivi ad N

**Esercizio 5**: Scrivere un ciclo che letti 10 numeri ne scriva il massimo.

**Esercizio 6**: Scrivere un ciclo che letti 10 numeri scriva la somma dei numeri il cui valore è compreso fra 10 e 20.

**Esercizio 7**: Scrivere un ciclo che letti due numeri N e M con N&lt;M, scriva tutti i numeri compresi tra N e M.

**Esercizio 8**: Scrivere un ciclo che letti due numeri N e M con N&lt;M, sommi tutti i numeri compresi tra N e M. Scrivere la somma ottenuta.

**Esercizio 9**: Scrivere un ciclo che letto un numero N scriva tutti i suoi divisori

**Esercizio 10**: Scrivere un ciclo che letto un numero N ne calcoli la radice quadrata intera (ovvero il massimo intero x tale che x<sup>2</sup>≤N)

**Esercizio 11**: Scrivere un ciclo che letto un numero N conti i suoi divisori e ne scriva il numero

**Esercizio 12**: Scrivere un ciclo che calcoli il fattoriale di un numero intero fornito dall’utente.

**Esercizio 13**: Scrivere un ciclo che letto un numero N permetta poi all’utente di inserire N numeri e provveda a calcolare la loro somma e la loro media e le scriva.

**Esercizio 14**: Scrivere un programma che legga due numeri, quindi chiede di inserire la somma. Fino a quando l’utente non inserisce la somma corretta, il programma scrive la frase “Errato: riprova”

**Esercizio 15**: Scrivere un programma che letto un numero positivo N determini il massimo intero K tale che la somma dei primi K interi sia minore o uguale a N.

Ad esempio, se N=20 allora K risulta 5, infatti

1 + 2 + 3 + 4 + 5 = 15 mentre

1 + 2 + 3 + 4 + 5 + 6 = 21

**Esercizio 16**: Scrivere un ciclo che continui a **leggere** valori interi digitati dall’utente e a sommarli fino all’immissione del quinto numero pari. Scrivere la somma ottenuta.

**Esercizio 17**: Trovare il minor numero di banconote da 100€, 50€, 10€, 5€, necessarie per pagare una assegnata cifra C multipla di 5.

**Esercizio 18:** scrivi un programma python che utilizzando due cicli for scriva la tabellina della addizione

**Esercizio 19:** scrivi un programma python che utilizzando due cicli for scriva la tabellina della moltiplicazione

**Esercizio 20:** scrivi un programma python che trovi i numeri primi compresi tra 1 e 100000.