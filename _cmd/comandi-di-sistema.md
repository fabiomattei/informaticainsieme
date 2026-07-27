---
title: 'Comandi di sistema e diagnostica'
date: '2026-07-26T09:50:00+02:00'
author: Fabio Mattei
layout: page
---

Questa pagina raccoglie i comandi più utili per ottenere informazioni sul sistema, sui processi in esecuzione e sull'utente collegato.

## whoami - chi sono

{% highlight shell %}
whoami
{% endhighlight %}

Mostra il nome dell'utente attualmente collegato, nel formato `computer\utente`.

## systeminfo - informazioni sul sistema

{% highlight shell %}
systeminfo
{% endhighlight %}

Mostra un report dettagliato su sistema operativo, hardware, aggiornamenti installati e molto altro.

## tasklist - elenca i processi in esecuzione

{% highlight shell %}
tasklist
{% endhighlight %}

Equivalente Windows del comando `ps` di Linux/macOS. Ogni processo ha un identificativo numerico, il **PID** (Process ID).

## taskkill - termina un processo

{% highlight shell %}
taskkill /pid 1234
{% endhighlight %}

Termina il processo con PID 1234. Si può anche indicare il nome del programma:

{% highlight shell %}
taskkill /im notepad.exe /f
{% endhighlight %}

L'opzione `/f` (force) forza la chiusura immediata del programma.

## chkdsk - controlla lo stato del disco

{% highlight shell %}
chkdsk C:
{% endhighlight %}

Analizza il disco alla ricerca di errori nel file system e nei settori.

## sfc - verifica l'integrità dei file di sistema

{% highlight shell %}
sfc /scannow
{% endhighlight %}

Controlla e ripara automaticamente eventuali file di sistema danneggiati o mancanti (richiede il Prompt eseguito come amministratore).

## shutdown - spegne o riavvia il computer

{% highlight shell %}
shutdown /s /t 0     spegne subito il computer
shutdown /r /t 0     riavvia subito il computer
shutdown /a          annulla uno spegnimento/riavvio programmato
{% endhighlight %}

`/t 0` indica di eseguire l'operazione senza attesa (0 secondi); aumentando il numero si può ritardare l'operazione.

## Eseguire il Prompt come amministratore

Alcuni comandi (come `sfc` o la modifica di file di sistema) richiedono privilegi elevati. Per aprire il Prompt come amministratore: cercarlo nel menu Start, tasto destro sull'icona e scegliere "Esegui come amministratore".
