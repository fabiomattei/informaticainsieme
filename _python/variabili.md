---
id: 202
title: Variabili
date: '2020-02-07T21:44:05+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=202'
---

Le variabili sono **contenitori di informazioni**. Al fine di creare o inizializzare una variabile Python si avvale dell’operatore di assegnamento indicato dal simbolo **=** in questo modo:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">x = 5
y = "John"
z = 3.564
```

</div>L’operando alla sinistra del simbolo = è il **nome della variabile**, l’operando alla destra è il **valore da conservare** all’interno della variabile.

In questo esempio vengono create tre variabili:

- una variabile x cui viene assegnato il valore 5
- una variabile y cui viene assegnato il valore “John”
- una variabile z cui viene assegnato il valore 3.564

## Nomi di variabile

Una variabile può avere un nome corto (come x e y) oppure un nome più descrittivo (nome, volume\_totale, area, altezza, cognome).

Nello scegliere il nome di una variabile Python dà molta libertà, ci sono però alcune regole da rispettare:

- Il nome di una variabile deve iniziare con una lettera o con il carattere trattino basso (underscore)
- Il nome di una variabile non può iniziare con un numero
- Il nome di una variabile può contenere soltanto simboli alfabetici, maiuscoli e minuscoli, numeri e trattino basso (A-z, 0-9, e \_ )
- I nomi di variabili sono case-sensitive, questo significo che lettere maiuscoli e minuscole vengono riconosciute come diverse. nome, Nome e NOME sono tre nomi di variabile diversi.

#### Variabili e calcoli

E’ possibile utilizzare le variabili per memorizzare i risultati di un calcolo. In questo caso l’operando alla sinistra del simbolo di = è il **nome della variabile**, l’operando alla destra **l’espressione da calcolare**.

Esempio:  
Patrizio va al mercato e compra 6 panini. Ciascun panino costa 2€. Quanto spenderà Patrizio?

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">numero_panini = 6
costo_panino = 2
costo_totale = numero_panini * costo_panino
```

</div>Osserviamo che:

- Alle variabili sono stati dati nomi significativi che seguono le regole elencate in precedenza
- Nelle variabili si memorizzano valori ma non le unità di misura
- Il simbolo \* in informatica è utilizzato per indicare la moltiplicazione