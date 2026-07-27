---
title: 'Amministrazione: utenti, gruppi e attività pianificate'
date: '2026-07-26T10:30:00+02:00'
author: Fabio Mattei
layout: page
---

I comandi di questa pagina servono a gestire gli **account utente** del computer e a **pianificare** l'esecuzione automatica di programmi o script. Sono comandi pensati per chi amministra un computer: molti richiedono di aprire il Prompt **come amministratore** (tasto destro sull'icona → "Esegui come amministratore").

## net user - gestire gli account utente

Per vedere l'elenco di tutti gli utenti presenti sul computer:

{% highlight shell %}
net user
{% endhighlight %}

Per vedere i dettagli di un singolo utente (gruppi a cui appartiene, scadenza password, ecc.):

{% highlight shell %}
net user Fabio
{% endhighlight %}

Per creare un nuovo utente locale:

{% highlight shell %}
net user Marco Password123 /add
{% endhighlight %}

Per cambiare la password di un utente esistente:

{% highlight shell %}
net user Marco NuovaPassword456
{% endhighlight %}

Per eliminare un utente:

{% highlight shell %}
net user Marco /delete
{% endhighlight %}

## net localgroup - gestire i gruppi

Gli utenti vengono organizzati in **gruppi**, che stabiliscono cosa possono fare. Il gruppo più importante è `Administrators` (o `Amministratori` in italiano), i cui membri possono modificare qualsiasi impostazione del sistema.

Per vedere tutti i gruppi disponibili:

{% highlight shell %}
net localgroup
{% endhighlight %}

Per vedere chi fa parte di un gruppo:

{% highlight shell %}
net localgroup Administrators
{% endhighlight %}

Per aggiungere un utente a un gruppo:

{% highlight shell %}
net localgroup Administrators Marco /add
{% endhighlight %}

Per rimuoverlo:

{% highlight shell %}
net localgroup Administrators Marco /delete
{% endhighlight %}

Attenzione: aggiungere un utente al gruppo `Administrators` gli dà pieno controllo del computer. Va fatto solo se ci si fida davvero di quell'utente.

## schtasks - pianificare l'esecuzione automatica

Il comando `schtasks` permette di dire a Windows di eseguire da solo un comando o uno script in un momento preciso (una volta, o ripetutamente), senza bisogno che nessuno prema Invio.

Per vedere le attività già pianificate:

{% highlight shell %}
schtasks /query
{% endhighlight %}

Per crearne una nuova, ad esempio uno script che parte ogni giorno alle 8:00:

{% highlight shell %}
schtasks /create /tn "Backup mattutino" /tr "C:\script\backup.bat" /sc daily /st 08:00
{% endhighlight %}

* `/tn` (task name) è il nome che diamo all'attività
* `/tr` (task run) è il comando o programma da eseguire
* `/sc daily` indica la frequenza (`daily`, ma anche `weekly`, `monthly`, `once`...)
* `/st` (start time) è l'orario di partenza

Per eliminare un'attività pianificata:

{% highlight shell %}
schtasks /delete /tn "Backup mattutino" /f
{% endhighlight %}

L'opzione `/f` (force) evita la richiesta di conferma.
