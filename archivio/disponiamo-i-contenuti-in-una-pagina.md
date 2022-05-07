---
id: 864
title: 'Disponiamo i contenuti in una pagina'
date: '2021-12-11T06:03:23+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=864'
---

Il W3C ha definito il **grid system** in modo da dare un sistema per disporre contenuti in una pagina web. Il sito [The grid system](http://www.thegridsystem.org) contiene molti esempi da cui si può prendere spunto.

Il problema che risolve è il seguente: dividere la pagina in sezioni in modo da dare una struttura logica e grafica alla pagina.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/grid-system-1024x815.png)</figure>Questo risultato si raggiunge utilizzando un tag **div** come contenitore e definendo le sottoaree attraverso i css.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="css" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">.container {
  display: grid;
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
```

</div>La prorietà css **grid-template-columns** e **grid-template-rows** mi permettono di definire il numero e le dimensioni di rige e colonne della griglia base. Le unità di misura per definire le dimensioni possono essere: percentuale, pixel, frazioni.

Una volta definita la griglia base devo disporre le aree al suo interno per creare per esempio qualcosa del genere.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/grid-system-struttura-1024x706.png)</figure><div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="css" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: auto;
  grid-template-areas: 
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}

.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

```

</div>In questo esempio la prima riga viene dedicata alla testata, le due colonne a sinistra ad una sezione di contenuti, segue uno spazio lasciato vuoto, una barra laterale ed in fine il piè di pagina.

L’html che si accompagna a questo css è il seguente:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="php" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"><div class="container" >
    <div class="item-a"></div>
    <div class="item-b"></div>
    <div class="item-c"></div>
    <div class="item-d"></div>
</div>
```

</div>Posso inserire all’interno dei divisor i contenuti che voglio essendo certo che finiranno nella giusta porzione di pagina.

Il grid sistem risolve un problema che per anni ha confuso i creatori di pagine web. E’ uno strumento che seppur tardivo si rivelò molto potente.