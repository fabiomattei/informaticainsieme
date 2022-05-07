---
id: 201
title: 'Come installare python su windows'
date: '2020-02-07T08:50:22+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/02/07/190-revision-v1/'
permalink: '/?p=201'
---

Scaricare python dal sito: <https://www.python.org/downloads/>

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Picture-1.png)</figure>Cliccare sul pulsante giallo “Download Python”

Mandare in esecuzione il pacchetto scaricato.

Installare Python avendo l’accortezza di selezionare “Add Python3.8 to PATH”

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Picture-2.png)</figure>Finita l’installazione assicurarsi che tra le variabili di ambiente sia presenta la variabile **Path**

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Picture-3-817x1024.png)</figure>Cliccare sulla variabile path quindi su “Modifica”.

Assicurarsi che siano presenti le diciture:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="sh" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">C:\Users\burat\AppData\Local\Programs\Python\Python38\Scripts\
C:\Users\burat\AppData\Local\Programs\Python\Python38\

```

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Picture-4.png)</figure># Per installare i pacchetti python necessari:

Accedere all prompt dei comandi come amministratore, a tal fine cliccare con il tasto destro sull’icona del prompt e poi cliccare su “**Esegui come amministratore**”

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Picture-5.png)</figure>Digitare il comando:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="sh" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">pip install arcade

```

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/Picture-6.png)</figure>Il sistema provvederà a scaricare i pacchetti da internet ed installarli nel sistema.

Se si dispone di python 3.7 o precedende digitare il comando:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="sh" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">pip install dataclasses

```

</div>