---
id: 112
title: 'La condizione'
date: '2020-02-04T14:57:13+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/02/04/105-autosave-v1/'
permalink: '/?p=112'
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
else:
    print("a e' piu' grande di b")
```

</div>