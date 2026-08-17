---
title: 'Variabili di ambiente'
date: '2026-07-26T09:30:00+02:00'
author: Fabio Mattei
layout: page
---

![Coppia nome/valore di una variabile di ambiente accessibile a più programmi](/images/cmd/variabili-di-ambiente/variabili-di-ambiente.svg){:class="aside-image"}

Le **variabili di ambiente** sono valori memorizzati dal sistema operativo e accessibili da qualsiasi programma o script. Vengono usate per configurare il comportamento del sistema (percorsi di ricerca, cartelle temporanee, ecc.).

## set - visualizza o imposta una variabile

Per vedere tutte le variabili di ambiente attive:

{% highlight shell %}
set
{% endhighlight %}

Per vedere il valore di una singola variabile:

{% highlight shell %}
set PATH
{% endhighlight %}

Per impostarne una nuova (valida solo per la sessione corrente del Prompt):

{% highlight shell %}
set NOME=Fabio
{% endhighlight %}

Per usare il valore di una variabile la si racchiude tra simboli `%`:

{% highlight shell %}
echo %NOME%
{% endhighlight %}

## Alcune variabili predefinite comuni

{% highlight shell %}
%PATH%          elenco delle cartelle in cui il sistema cerca i programmi eseguibili
%USERNAME%      nome dell'utente attualmente collegato
%USERPROFILE%   cartella personale dell'utente (es. C:\Users\Utente)
%WINDIR%        cartella di installazione di Windows (es. C:\Windows)
%TEMP%          cartella dei file temporanei
{% endhighlight %}

## echo - stampa un testo (o una variabile) a schermo

{% highlight shell %}
echo Ciao mondo
{% endhighlight %}

`echo` viene usato anche per attivare o disattivare la visualizzazione dei comandi eseguiti in uno script batch:

{% highlight shell %}
echo off     nasconde i comandi eseguiti, mostra solo il loro output
echo on      torna a mostrare anche i comandi eseguiti
{% endhighlight %}

## setx - imposta una variabile in modo permanente

A differenza di `set`, il comando `setx` salva la variabile anche dopo la chiusura del Prompt:

{% highlight shell %}
setx NOME "Fabio"
{% endhighlight %}

Nota: dopo `setx` occorre aprire un nuovo Prompt dei comandi perché la modifica abbia effetto.

## set /p - chiedere un valore all'utente

Finora abbiamo sempre assegnato un valore a una variabile scrivendolo noi nello script. Con `set /p` possiamo invece far scrivere il valore direttamente a chi usa lo script, proprio come farebbe `input()` in Python:

{% highlight shell %}
@echo off
set /p NOME=Come ti chiami? 
echo Ciao, %NOME%!
{% endhighlight %}

Quando lo script arriva a questa riga, si ferma e aspetta che l'utente scriva qualcosa e prema Invio. Il testo dopo il segno `=` (qui "Come ti chiami? ") è solo una domanda mostrata a schermo, non fa parte del valore memorizzato.

Questo comando è quello che serve per rendere davvero interattivo uno script batch, ad esempio per creare un piccolo programma che saluta chi lo usa o che chiede una scelta prima di continuare.

## set /a - fare calcoli

Con l'opzione `/a` il comando `set` esegue un calcolo aritmetico invece di limitarsi a copiare un testo:

{% highlight shell %}
@echo off
set /a RISULTATO=5+3
echo Il risultato è %RISULTATO%
{% endhighlight %}

Si possono usare i normali operatori matematici:

{% highlight shell %}
set /a SOMMA=4+2
set /a DIFFERENZA=10-4
set /a PRODOTTO=6*7
set /a DIVISIONE=20/4
set /a RESTO=20%%3
{% endhighlight %}

Attenzione: `set /a` lavora solo con **numeri interi** (niente virgola), e per calcolare il resto di una divisione dentro uno script batch il simbolo di percentuale va raddoppiato (`%%3`), esattamente come si fa nei cicli `for`.

## Esempio: una piccola calcolatrice

Combinando `set /p`, `set /a` e `if` (visto nella pagina sugli [script batch]({{ site.baseurl }}{% link _cmd/script-batch.md %}.html)) si può scrivere una vera calcolatrice, che chiede due numeri e l'operazione da eseguire:

{% highlight shell %}
@echo off
set /p NUM1=Inserisci il primo numero: 
set /p NUM2=Inserisci il secondo numero: 
set /p OP=Operazione (+, -, *, /): 

if "%OP%"=="+" set /a RISULTATO=%NUM1%+%NUM2%
if "%OP%"=="-" set /a RISULTATO=%NUM1%-%NUM2%
if "%OP%"=="*" set /a RISULTATO=%NUM1%*%NUM2%
if "%OP%"=="/" set /a RISULTATO=%NUM1%/%NUM2%

echo Risultato: %RISULTATO%
pause
{% endhighlight %}

Salvando questo testo in un file `calcolatrice.bat` ed eseguendolo, si ottiene un piccolo programma completo: chiede i dati all'utente (`set /p`), decide cosa fare in base alla risposta (`if`) e infine calcola il risultato (`set /a`). È un buon esempio di come, mettendo insieme pochi comandi semplici, si possa già costruire qualcosa di utile.
