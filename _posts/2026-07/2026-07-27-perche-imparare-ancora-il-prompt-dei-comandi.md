---
layout: post
title:  "Perché conviene ancora imparare il Prompt dei comandi (anche nel 2026)"
date:   2026-07-27 00:00:00
categories: blog cmd windows
excerpt: In un mondo di interfacce grafiche, touchscreen e assistenti basati sull'intelligenza artificiale, la riga di comando di Windows sembra un residuo del passato. Eppure resta uno strumento sorprendentemente utile, ed è ancora uno dei modi migliori per iniziare a capire davvero come funziona un computer.
author: Fabio
tags: cmd windows shell informatica
---

Aprire il Prompt dei comandi su un computer Windows del 2026 significa ritrovarsi davanti a una finestra nera con un cursore lampeggiante, praticamente identica a quella che si poteva vedere trent'anni fa su MS-DOS. In un'epoca di interfacce animate, assistenti vocali e touchscreen, sembrerebbe il posto più improbabile in cui investire tempo per imparare qualcosa di nuovo. Eppure quella finestra nera non se n'è mai andata: ogni singola versione di Windows, compresa quella che gira oggi sul computer dell'utente più comune, la include ancora. Non è nostalgia: è che certi problemi la riga di comando li risolve ancora meglio di qualsiasi interfaccia grafica.

Chiunque abbia mai dovuto rinominare cento file con lo stesso schema, copiare una cartella intera preservando la sua struttura, o capire perché un programma non risponde più, sa quanto possa essere lento farlo cliccando in giro tra le finestre. Un comando come

{% highlight shell %}
robocopy C:\progetti D:\backup /mir
{% endhighlight %}

fa in una riga quello che altrimenti richiederebbe minuti di trascinamenti e conferme. Lo stesso vale per trovare un processo che sta consumando risorse (`tasklist | find "nome"`), o per cercare una parola dentro decine di file di testo (`findstr /s "errore" *.log`). Non è questione di essere "più smanettoni": è che alcune operazioni, per loro natura, si descrivono più velocemente a parole che a colpi di mouse.

C'è poi un motivo meno pratico e più formativo. Quando si usa un'interfaccia grafica, il sistema operativo nasconde deliberatamente la sua complessità dietro icone e finestre. È comodo, ma rende invisibili concetti che stanno alla base dell'informatica: cos'è un percorso di file, cosa significa che un processo ha un identificativo numerico, come il sistema decide quale programma eseguire quando si digita un nome. La riga di comando toglie quel velo: digitare `dir`, `cd`, `tasklist` costringe a ragionare esplicitamente su cartelle, processi e percorsi, invece di darli per scontati. Per chi studia informatica, è un esercizio che vale più di molte lezioni teoriche.

Ed è anche, senza che lo si noti troppo, un primo passo verso la programmazione vera e propria. Uno script batch — un semplice file `.bat` con dentro una sequenza di comandi — è a tutti gli effetti un piccolo programma: ha variabili, ha istruzioni condizionali (`if`), ha cicli (`for`), e può persino chiedere dati all'utente e fare calcoli:

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

Poche righe, ma dentro ci sono già tutti gli ingredienti della programmazione: input dell'utente, condizioni, calcoli, output. Per uno studente che si affaccia per la prima volta a questi concetti, uno script batch è un punto di partenza più immediato di quanto sembri, e rende meno intimidatorio il passo successivo verso un linguaggio come Python.

C'è infine un motivo molto più attuale. Sempre più spesso capita di chiedere a un assistente basato sull'intelligenza artificiale di scrivere uno script per automatizzare un'operazione noiosa: rinominare file, fare un backup, cercare del testo in decine di documenti. L'assistente risponderà quasi certamente con qualche riga di comando o con un piccolo file batch. Chi conosce almeno le basi della riga di comando può leggere quel suggerimento, capire davvero cosa farà prima di eseguirlo, e correggerlo se qualcosa non torna. Chi non le conosce, invece, si trova costretto a fidarsi ciecamente, il che, quando un comando può cancellare file o modificare il sistema, non è mai una buona idea. Saper leggere una riga di comando è, in un certo senso, una forma di alfabetizzazione digitale che diventa ancora più preziosa nell'epoca degli assistenti automatici.

Per chi volesse partire da zero, sul sito è disponibile una guida completa dedicata proprio alla shell di Windows: dalla navigazione tra le cartelle, alla gestione dei file, fino agli script batch, alle pipe e ai comandi di amministrazione. [Vai alla sezione Shell Windows (CMD)]({{ site.baseurl }}/paginemenu/cmd.html)
