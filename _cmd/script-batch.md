---
title: 'Gli script batch (file .bat)'
date: '2026-07-26T09:40:00+02:00'
author: Fabio Mattei
layout: page
---

![File .bat con comandi eseguiti in sequenza numerata dal terminale](/images/cmd/script-batch/script-batch.svg){:class="aside-image"}

Un file **batch** è un file di testo con estensione `.bat` (o `.cmd`) che contiene una sequenza di comandi da eseguire uno dopo l'altro, esattamente come se fossero digitati a mano nel Prompt dei comandi. È l'equivalente Windows degli script di shell in ambiente Linux.

## Il primo script batch

Creiamo un file chiamato `saluto.bat` con il Blocco note, contenente:

{% highlight shell %}
@echo off
echo Ciao, benvenuto!
pause
{% endhighlight %}

* `@echo off` disattiva la visualizzazione dei comandi eseguiti (la `@` nasconde anche questa stessa riga)
* `pause` mette in pausa lo script mostrando "Premere un tasto per continuare . . ." finché non si preme un tasto

Per eseguirlo basta digitarne il nome nel Prompt (posizionandosi nella cartella giusta):

{% highlight shell %}
saluto.bat
{% endhighlight %}

## Parametri di uno script

Uno script batch può ricevere parametri quando viene lanciato, richiamabili con `%1`, `%2`, ecc.:

{% highlight shell %}
@echo off
echo Il primo parametro è: %1
echo Il secondo parametro è: %2
{% endhighlight %}

Se lo script si chiama `mostra.bat`, lo si lancia così:

{% highlight shell %}
mostra.bat Ciao Mondo
{% endhighlight %}

## if - istruzioni condizionali

{% highlight shell %}
@echo off
if "%1"=="" (
    echo Non hai inserito alcun parametro!
) else (
    echo Hai inserito: %1
)
{% endhighlight %}

## for - cicli

Il comando `for` permette di ripetere un'azione, ad esempio per ogni file di una cartella:

{% highlight shell %}
@echo off
for %%f in (*.txt) do echo Trovato il file: %%f
{% endhighlight %}

Da notare che dentro uno script batch il simbolo di percentuale nei cicli `for` va raddoppiato (`%%f` invece di `%f`, che si usa solo digitando il comando direttamente nel Prompt).

## goto - salta a un'etichetta

{% highlight shell %}
@echo off
echo Inizio
goto fine
echo Questa riga non verrà mai eseguita
:fine
echo Fine
{% endhighlight %}

Le etichette si definiscono facendole precedere dai due punti (`:fine`) e permettono di realizzare cicli e salti condizionati, in modo simile al `goto` di altri linguaggi.

## Combinare più comandi sulla stessa riga

{% highlight shell %}
comando1 && comando2      esegue comando2 solo se comando1 ha avuto successo
comando1 || comando2      esegue comando2 solo se comando1 ha fallito
comando1 & comando2       esegue comando2 subito dopo comando1, in ogni caso
{% endhighlight %}

## errorlevel - capire se un comando è andato a buon fine

Quando un comando finisce, Windows si "ricorda" se è andato tutto bene oppure no, usando un numero chiamato **codice di uscita**. Questo numero si può leggere subito dopo con la variabile speciale `%errorlevel%`.

Per convenzione: `0` significa che tutto è andato bene, un numero diverso da `0` significa che c'è stato un problema.

{% highlight shell %}
@echo off
copy appunti.txt D:\backup\
if %errorlevel%==0 (
    echo Copia riuscita!
) else (
    echo Qualcosa è andato storto...
)
{% endhighlight %}

È molto utile negli script più lunghi, per fermarsi (o avvisare) appena qualcosa non funziona come previsto, invece di continuare come se niente fosse.

## call - richiamare un altro script batch

Se dentro uno script si scrive semplicemente il nome di un altro file `.bat`, quando quel file finisce **lo script che lo ha chiamato non riprende più**: il controllo resta lì. Per richiamare un altro script e poi tornare a continuare quello di partenza si usa `call`:

{% highlight shell %}
@echo off
echo Inizio dello script principale
call altro-script.bat
echo Questa riga viene eseguita anche dopo altro-script.bat
{% endhighlight %}

`call` è utile per dividere uno script lungo in più file più piccoli, magari uno per ogni operazione, e poi richiamarli da un unico script "principale".

## pushd e popd - spostarsi e tornare indietro

Questi due comandi aiutano quando, dentro uno script, bisogna spostarsi temporaneamente in un'altra cartella e poi tornare esattamente dove si era prima.

{% highlight shell %}
@echo off
echo Mi trovo in una cartella
pushd D:\backup
echo Ora sono dentro D:\backup
dir
popd
echo Sono tornato nella cartella di partenza
{% endhighlight %}

* `pushd` memorizza la cartella corrente e si sposta in quella indicata
* `popd` torna alla cartella memorizzata da `pushd`

È come lasciare un segnalibro prima di spostarsi, per poter tornare facilmente al punto di partenza.
