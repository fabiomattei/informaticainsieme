---
id: 114
title: 'La condizione'
date: '2020-02-04T15:01:19+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/02/04/105-revision-v1/'
permalink: '/?p=114'
---

#### Il costrutto if

  
Il costrutto **if** ci consente di cambiare la sequenza logica di istruzioni da eseguire in un programma. L’istruzione successiva viene eseguita se la condizione risulta verificata.

**Esercizio 1:** copia il seguente codice nell’editor. Una volta finito fallo eseguire.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">a = 33
b = 200
if b > a:
   print("b e' maggiore di a")
```

</div>**Esercizio 2:** copia il seguente codice nell’editor. Una volta finito fallo eseguire.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">a = 33
b = 200
if b > a:
print("b e' maggiore di a") # questo codice è errato dato che manca l'indentazione
```

</div>In Python l’indentazione (gli spazi che nell’esempio 1 precedono l’istruzione print) indica l’appartenenza di un codice ad un blocco di codice. Dato che bisogna distinguere tra quali istruzioni eseguire in caso la condizione venga verificata oppure no, chi ha inventato il linguaggio ha deciso di utilizzare l’indentazione a tale scopo. Dunque nel primo esempio è chiaro per il computer che deve eseguire l’istruzione print() se b &gt; a, nel secondo esempio, mancando indentazione non è chiaro, quindi il computer ci dà errore.

####  Operatori di confronto

Sono gli operatori che possiamo utilizzare all’interno di una condizione.

- Uguale: a == b
- Diverso: a != b
- Minore: a &lt; b
- Minore o uguale: a &lt;= b
- Maggiore: a &gt; b
- Maggiore o uguale: a &gt;= b

####  Il costrutto elif

Importante per dare una alternativa in caso la proposizione contenuta nel primo if non venga verificata.

**Esercizio 3:** copia il seguente codice nell’editor. Una volta finito fallo eseguire.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">a = 33
b = 33
if b > a:
    print("b e' piu' grande di a")
elif a == b:
    print("a e b sono uguali")
```

</div>#### Il costrutto else

Nel caso nessuna delle proposizione contenute nelle condizioni precedenti sia stata verificata, si esegue il codice contenuto nel blocco else:

**Esercizio 4:** copia il seguente codice nell’editor. Una volta finito fallo eseguire.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">a = 33
b = 33
if b > a:
    print("b e' piu' grande di a")
elif a == b:
    print("a e b sono uguali")
else:
    print("a e' piu' grande di b")
```

</div>**Esercizio 5:** scrivi un programma che lette due stringhe di testo ne scriva la prima in ordine alfabetico

**Esercizio 6:** scrivi un programma che letti due numeri ne scriva il maggiore

**Esercizio 7:** scrivi un programma che letto un numero intero determini se è pari o dispari (utilizzare l’operatore resto: %)

**Esercizio 8:** Dati 4 numeri in input determinare se la somma dei primi due è minore o uguale alla somma del terzo e del quarto

**Esercizio 9:** Un’automobile percorre 20 km con un litro di benzina. Calcolare la spesa necessaria a percorrere 100 km. Se la spesa è maggiore di €100, applicare uno sconto del 5%

**Esercizio 10:** Letti i lati di un triangolo determinare se è scaleno, isoscele o equilatero

**Esercizio 11:** Letti due numeri naturali A e B, sottrarre il più piccolo dal più grande.

**Esercizio 12:** Letti due numeri determinare se sono entrambi compresi tra 100 e 200

**Esercizio 13:** Letto un numero intero, trovare il suo valore assoluto.

**Esercizio 14:** Letti due numeri interi A e B verificare se A è il quadrato di B

**Esercizio 15:** Un’azienda elettrica ha stabilito le seguenti tariffe:

<figure class="wp-block-table">| KW/H |  |
|---|---|
| 0 – 500 | 20 |
| 501 – 1000 | 20 + 0,08 per ogni KW/H oltre i 500 |
| 1001 – oltre | 20 + 0,05 per ogni KW/H oltre i 1000 |

</figure>Scrivere un programma che letto il consumo mensile calcoli e stampi l’importo della bolletta.