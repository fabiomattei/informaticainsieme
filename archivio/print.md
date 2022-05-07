---
id: 216
title: Print
date: '2020-02-07T22:32:45+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=216'
---

La funzione print stampa un messaggio sulla console sullo schermo.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">print("Hello World")

```

</div>La funzione print() stampa un messaggio specifico sullo schermo.

Il messaggio può essere una stringa di testo o un qualsiasi altro tipo di variabile che verrà automaticamente convertito in stringa dal linguaggio prima di essere scritto sullo schermo.

#### Sintassi

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">print(oggetto(i), sep=separatore, end=fine)

```

</div><figure class="wp-block-table">| Parametro | Descrizione |
|---|---|
| *oggetto(i)* | Qualsiasi oggetto, tanti quanti ne vuoi. Sarà convertito in stringa prima di essere scritto sullo schermo. |
| sep=’*separator*e’ | Opzionale. Specifica come separare gli oggetti. Il valore di default è ‘ ‘ |
| end=’*fine*‘ | Opzionale. Specifica il simbolo da scrivere dopo aver scritto tutti gli oggetti. Il valore di default è ‘\\n’ (line feed) |

</figure>#### Esempi

Digita e manda in esecuzione le seguenti istruzioni:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"> print("Ciao", "Come stai?") 
 print("Mela", "Ciliegia", "Pesca", sep="---")
 print("Trota", "Salmone", "Tonno", end=" ")
 print("Aquila", "Falco", "Cardellino", sep="#", end="@")
 
```

</div>Leggi cosa viene scritto nella console e mettilo in relazione con i comandi che hai digitato.