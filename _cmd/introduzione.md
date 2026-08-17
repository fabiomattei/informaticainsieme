---
title: 'Introduzione al Prompt dei comandi'
date: '2026-07-26T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Struttura di un comando nel Prompt: comando, parametro e opzione](/images/cmd/introduzione/introduzione.svg){:class="aside-image"}

Il **Prompt dei comandi** (in inglese *Command Prompt*, eseguibile `cmd.exe`) è l'interprete a riga di comando di Windows. Permette di impartire istruzioni al sistema operativo digitando comandi testuali, senza passare dall'interfaccia grafica.

È l'erede del **MS-DOS** (il sistema operativo a riga di comando che Microsoft distribuiva prima di Windows) e ne conserva ancora oggi molti comandi e molte convenzioni.

## Come aprire il Prompt dei comandi

Esistono diversi modi per aprirlo:

* Premere `Windows + R`, digitare `cmd` e premere Invio.
* Digitare "prompt dei comandi" nella barra di ricerca di Windows.
* Dalla cartella desiderata in Esplora file, digitare `cmd` nella barra dell'indirizzo e premere Invio: si aprirà il prompt già posizionato in quella cartella.

## La struttura di un comando

Un comando è generalmente composto da tre parti:

{% highlight shell %}
comando parametro /opzione
{% endhighlight %}

Ad esempio:

{% highlight shell %}
dir /w
{% endhighlight %}

Qui `dir` è il comando, `/w` è un'opzione (o *switch*) che ne modifica il comportamento. Le opzioni in ambiente Windows iniziano quasi sempre con `/` (a differenza della shell Linux, che usa `-` o `--`).

## Il prompt

La riga su cui si digita, terminante con il simbolo `>`, si chiama **prompt** e mostra sempre la cartella di lavoro corrente:

{% highlight shell %}
C:\Users\Utente>
{% endhighlight %}

Tutti i comandi che vengono digitati agiscono, salvo diversa indicazione, a partire da questa cartella.

## Un primo comando: cls

Per pulire lo schermo da tutto ciò che è stato scritto finora si usa:

{% highlight shell %}
cls
{% endhighlight %}

## Come ottenere aiuto

Quasi tutti i comandi hanno una guida integrata, richiamabile aggiungendo `/?`:

{% highlight shell %}
dir /?
{% endhighlight %}

Per un elenco di tutti i comandi disponibili si può digitare semplicemente:

{% highlight shell %}
help
{% endhighlight %}
