---
id: 128
title: 'La ricorsione'
date: '2020-02-04T15:45:02+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=128'
---

Problema: devo lavare una pila di 15 piatti.   
Soluzione:

- lavo un piatto;
- devo lavare 14 piatti.

Sembra un ragionamento banale ma il suo scopo è quello di sottolineare come un problema possa essere scomposto utilizzando la sua stessa definizione. Il problema devo lavare 15 piatti viene scomposto nel problema lavo un piatto e devo lavare 14 piatti. Generalizzando possiamo scrivere che devo lavare n piatti viene scomposto nel problema lavo un piatto e devo lavare n – 1 piatti.

La ricorsione (in inglese recursion) è una tecnica di programmazione molto potente che sfrutta l’idea di lavorare sulla definizione stessa del problema che stiamo risolvendo al fine di risolverlo attraverso l’algoritmo più semplice che si possa immaginare. Contrariamente a quanto si possa intuitivamente pensare, scrivere algoritmi semplici è estremante complesso, molti programmatori professionisti non sono in grado di applicare questa tecnica.

#### Ragioniamo con i numeri 

Supponiamo di voler calcolare la somma dei primi n numeri interi. Quello che facciamo consiste nel lavorare sulla definizione stessa del problema. Ragioniamo sui numeri, calcoliamo la somma dei primi 4 numeri interi: *1 + 2 + 3 + 4 = 10*

Se immaginiamo di aver definito una funzione *sommainteri(n)* dove n rappresenta il numero di interi da sommare, possiamo scrivere:

*sommainteri(4) = 1 + 2 + 3 + 4 = 10*

Ma questo equivale a scrivere:

*(1 + 2 + 3) + 4 = sommainteri(3) + 4*

Come abbiamo ragionato? Abbiamo lavorato sulla definizione stessa della funzione decomponendola in due parti:

*sommainteri(n) = sommainteri(n − 1) + n*

Questa relazione è valida fintantoché n &gt; 0. Parafrasando in Python:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">def sommainteri(n):
    if n > 0:
        return n + sommainteri(n-1) 
    else:
        return 0
```

</div>La soluzione appena trovata presenta un aspetto interessante: per risolvere il problema ci siamo basati sul poter risolvere lo stesso problema per un numero più piccolo. Questo approccio viene definito ricorsivo.

Un algoritmo ricorsivo per la risoluzione di un dato problema deve essere definito nel modo seguente:

- passo base: si definisce come risolvere il problema quando questo ha dimensione minima e può essere risolto in maniera estremamente semplice;
- passo ricorsivo: si definisce come ottenere la soluzione del problema come composizione di un problema analogo ma di dimensione inferiore e una operazione semplice.

#### Ricorsione Diretta 

Una funzione si dice ricorsiva quando all’interno della propria definizione compare una chiamata diretta a se stessa. Questa forma di ricorsione si chiama ricorsione diretta.

Un esempio di ricorsione diretta è la funzione per calcolare il fattoriale di un numero n:

Osserviamo che:

*n! = n ∗ (n − 1) ∗ … ∗ 2 ∗ 1 = n ∗ (n − 1)!*

Quindi la definizione induttiva del fattoriale è:

- (passo base) 0! = 1
- (passoricorsivo)n!=n∗(n−1)!(sen&gt;0).

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">def fattoriale(n): 
    if n == 0:
        return 1
    else:
        return n * fattoriale(n-1)
```

</div>Condizioni come (n == 1) si chiamano clausole di chiusura perché garantiscono che la ricorsione termini.

Esistono due requisiti che sono basilari per essere sicuri che la ricorsione funzioni:

- ogni invocazione ricorsiva deve semplificare in qualche modo l’elaborazione;
- devono esistere casi speciali che gestiscano in modo diretto le elaborazioni più semplici.

Occorre però fare molta attenzione: cosa succede se si calcola il fattoriale di -1? La clausola di chiusura non sarebbe mai verificata e il sistema andrebbe in un loop infinito. Per evitare che ciò accada facciamo una piccola modifica.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">def fattoriale(n): 
    if n == 0:
        return 1
    elif n < 1:
        print("Errore nell'input")
    else:
        return n * fattoriale(n-1)
```

</div>In questo modo la funzione fattoriale riesce sempre a concludere la propria elaborazione.

#### Ricorsione indiretta 

Si parla di ricorsione indiretta quando nella definizione di un metodo compare la chiamata ad un altro metodo il quale direttamente o indirettamente chiama il metodo iniziale.

Un esempio di ricorsione indiretta

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">def numero_pari(n): 
    if x == 0:
        return True
    else:
        return numero_dispari(n-1) 

def numero_dispari(x):
    return not numero_pari(n)
```

</div>Possiamo notare come la funzione numero\_pari abbia al suo interno la clausola di chiusura e la chiamata alla funzione numero\_dispari. Quest’ultima ha al suo interno la chiamata alla funzione numero\_pari.

#### Ricorsione Multipla 

Una funzione implementa una ricorsione multipla quando al suo interno compare, almeno due volte, la chiamata a se stessa.

Un classico esempio di ricorsione multipla è l’implementazione dei numeri di Fibonacci, la cui definizione è riportata sotto:

- fib(0)=0
- fib(1)=1
- fib(n)=fib(n−1)+fib(n−2) (se n&gt;1)

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">def fib(n):
    if n<0:
        print("Errore nell'input") 
    elif n==0:
        return 0
    elif n==1: 
        return 1
    else:
        return fib(n-1)+fib(n-2)
```

</div>Notare che, come per il fattoriale, la funzione è definita solo su interi non negativi.

#### La ricorsione e le liste 

È possibile utilizzare algoritmi ricorsivi per operare sulle liste. Se per esempio volessimo sommare tutti gli i numeri contenuti in una lista potremmo operare nel seguente modo:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">numeri = [6, 1, 7, 3]
def s_lista(lista): 
    if len(lista)==0:
        return 0
    else:
        return lista[0] + s_lista(lista[1:])
```

</div>- passo base: se la lista è vuota la somma dei numeri al suo interno è 0;
- passo ricorsivo: se la lista non è vuota la somma dei numeri al suo interno è pari al primo numero in lista cui va sommata il risultato della somma della sottolista ottenuta togliendo alla lista il primo numero.

Dato che una stringa altro non è che una lista di caratteri, è possibile operare su questa allo stesso modo.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">def reverse(string): 
    if len(string) == 0:
        return string
    else:
        return reverse(string[1:]) + string[0]
```

</div>### Esercizi 

**Esercizio 1:**

Scrivere una funzione ricorsiva che controlli se una stringa è palindroma (ovvero se “rigirandola” non cambia, es. “ossesso” è palindroma).

Esempi di frasi palindrome:

I verbi brevi (“iverbibrevi”) Aceto nell’enoteca (“acetonellenoteca”) I topi non avevano nipoti (“itopinonavevanonipoti”)

Definizione ricorsiva di palindromicità:

- Una stringa nulla è palindroma. Esempio: “”.
- Una stringa di un carattere è palindroma. Esempio: “a”.
- Una stringa aventi il primo e l’ultimo carattere uguali e la sottostringa nel mezzo è palindroma, è palindroma. Esempio: “ossesso”.

**Esercizio 2:**

Scrivere una funzione ricorsiva che analizzando una stringa in modo ricorsivo ne estragga, scrivendole in output (print), le sole lettere vocali.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">analizzanda = input("Digita la stringa da analizzare: ")
vocali = ['a', 'e', 'i', 'o', 'u']

def estrai_vocali(analizzanda, vocali):
    pass
    # scrivi tu questa funzione
```

</div>**Esercizio 3:**

Scrivere una funzione ricorsiva che calcoli la somma dei cifre contenute in un numero. Es. f(325) = 10

Soluzione:   
 (Caso Base) Se le cifre sono finite allora la somma delle sue cifre è zero   
 (Passo generico) Se il numero è composto da tante cifre allora la somma delle sue cifre è data dalla somma della prima cifra più la somma delle cifre seguenti.

**Esercizio 4:**

Creare una funzione ricorsiva per calcolare una funzione definita così:  
 per m&gt;0 allora f(n,m) = 1+f(n,m-1)  
 per m=0 allora f(n,m) = n  
 Una volta implementata, provarla e dire cosa calcola la funzione.

**Esercizio 5:**

Scrivere il codice di una funzione ricorsiva f(n) che restituisce 0 nel caso n sia dispari, 1+f(n/2) altrimenti.

**Esercizio 6:**

Scrivere il codice di una funzione ricorsiva int f(int n) che restituisce quante coppie di cifre uguali in posizioni adiacenti ci sono nel numero n, nel caso n sia negativo restituisce 0.  
*Ad es: f(551122) restituisce 3, f(5122) restituisce 1, f(9) restituisce 0.*

**Esercizio 7:**

Scrivere una funzione ricorsiva POT(n) per il calcolo dei numeri F(n) definiti dalle seguenti relazioni:

F(1) = 2   
F(n)=2F(n−1) n≥2

**Esercizio 7:**

Scrivere una funzione ricorsiva che, avendo in input una lista di n interi, dia in output il numero degli elementi positivi della lista.