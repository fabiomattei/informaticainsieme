---
title: 'Gestione dei processi in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Ogni programma in esecuzione su Linux è un **processo**, identificato da un numero univoco chiamato **PID** (Process ID). Il terminale offre diversi strumenti per osservare e controllare i processi in esecuzione.

### Vedere i processi attivi

{% highlight shell %}
ps aux         # elenca tutti i processi in esecuzione sul sistema
{% endhighlight %}

Le colonne principali mostrano l'utente proprietario del processo, il PID, l'uso di CPU e memoria, e il comando che lo ha avviato.

Per un monitoraggio in tempo reale, aggiornato automaticamente:

{% highlight shell %}
top            # monitor interattivo dei processi, ordinato per consumo di risorse
htop           # versione più leggibile e a colori di top (va installata a parte)
{% endhighlight %}

### Terminare un processo

Ogni processo può essere terminato inviandogli un **segnale**, tramite il comando `kill` seguito dal PID:

{% highlight shell %}
kill 1234           # invia il segnale di terminazione "gentile" (SIGTERM)
kill -9 1234         # invia il segnale di terminazione forzata (SIGKILL)
{% endhighlight %}

`SIGTERM` chiede al programma di chiudersi in modo ordinato (salvando eventuali dati); `SIGKILL` lo interrompe immediatamente, senza possibilità per il programma di reagire. Va usato solo quando `SIGTERM` non ha effetto.

Per terminare un processo conoscendone solo il nome:

{% highlight shell %}
pkill firefox
{% endhighlight %}

### Processi in primo piano e in background

Quando si avvia un comando dal terminale, questo occupa il terminale stesso finché non termina (**foreground**). Aggiungendo `&` al termine del comando, questo viene invece avviato in **background**, lasciando il terminale libero:

{% highlight shell %}
lungo_processo &
{% endhighlight %}

Altri comandi utili per gestire i processi in corso:

{% highlight shell %}
jobs           # elenca i processi in background nella sessione corrente
fg             # riporta l'ultimo processo in background in primo piano
bg             # riprende in background un processo sospeso
{% endhighlight %}

Un processo in primo piano può essere sospeso (non terminato) con `Ctrl+Z`, per poi essere ripreso con `fg` o `bg`.

### Il processo init e systemd

Il primo processo avviato dal kernel all'accensione, con PID 1, è responsabile di avviare tutti gli altri. Sulla maggior parte delle distribuzioni moderne questo ruolo è svolto da **systemd**, che gestisce anche i **servizi** (programmi che girano in background senza interazione diretta, come un server web):

{% highlight shell %}
systemctl status ssh     # mostra lo stato di un servizio
sudo systemctl start ssh  # avvia un servizio
sudo systemctl stop ssh   # ferma un servizio
sudo systemctl enable ssh # abilita l'avvio automatico del servizio al boot
{% endhighlight %}

### I segnali in dettaglio

`kill` non "uccide" necessariamente un processo: invia un **segnale**, un messaggio a cui il processo può reagire in modo diverso a seconda di come è stato programmato. I segnali più comuni:

* **SIGTERM (15)** — richiesta di terminazione "gentile": il processo può salvare i propri dati prima di chiudersi. È il segnale predefinito di `kill`.
* **SIGKILL (9)** — terminazione forzata e immediata, gestita direttamente dal kernel: il processo non può intercettarla né reagire.
* **SIGHUP (1)** — storicamente inviato quando il terminale da cui è partito un processo si chiude; molti servizi lo interpretano oggi come richiesta di ricaricare la propria configurazione senza riavviarsi.
* **SIGSTOP** e **SIGCONT** — sospendono e riprendono un processo, corrispondenti a `Ctrl+Z` e `bg`/`fg`.

{% highlight shell %}
kill -l              # elenca tutti i segnali disponibili con il loro numero
kill -HUP 1234        # invia SIGHUP al processo con PID 1234
{% endhighlight %}

### La priorità dei processi: nice e renice

Ogni processo ha un valore di **priorità** (*niceness*) che indica quanto è disposto a "cedere il passo" ad altri processi nell'uso della CPU, su una scala da -20 (massima priorità) a 19 (minima priorità). Un valore di niceness più alto significa che il processo è più "gentile" verso gli altri, cioè meno prioritario:

{% highlight shell %}
nice -n 10 ./calcolo_lungo.sh    # avvia un processo con priorità ridotta
renice 5 -p 1234                  # cambia la priorità di un processo già in esecuzione
{% endhighlight %}

Solo l'utente root può assegnare priorità negative (più alte del normale); un utente normale può solo ridurre la priorità dei propri processi, mai aumentarla.

### Pianificare l'esecuzione di comandi: cron

**cron** è il servizio che esegue comandi automaticamente a intervalli prestabiliti, ad esempio ogni notte o ogni ora. Le pianificazioni si scrivono nel **crontab**, un file personale per ogni utente:

{% highlight shell %}
crontab -e        # apre il crontab dell'utente corrente per modificarlo
crontab -l         # elenca le pianificazioni attive
{% endhighlight %}

Ogni riga del crontab ha la forma:

{% highlight shell %}
minuto ora giorno_mese mese giorno_settimana comando
{% endhighlight %}

Ad esempio, per eseguire uno script di backup ogni giorno alle 3:30 del mattino:

{% highlight shell %}
30 3 * * * /home/fabio/backup.sh
{% endhighlight %}

Gli asterischi indicano "ogni valore possibile" per quel campo; per un'esecuzione una tantum a un orario specifico (invece che ricorrente), si usa il comando `at` al posto di cron.

### I log di sistema

I processi di sistema scrivono il proprio andamento nei **log**, storicamente raccolti in file di testo dentro `/var/log`. Sulle distribuzioni moderne, che usano systemd, i log sono raccolti in un formato centralizzato consultabile con:

{% highlight shell %}
journalctl                    # mostra tutti i log raccolti da systemd
journalctl -u ssh              # mostra solo i log relativi al servizio ssh
journalctl -f                   # segue i nuovi log in tempo reale, come "tail -f"
{% endhighlight %}

Consultare i log è spesso il primo passo per capire perché un servizio non si è avviato correttamente o ha smesso di funzionare.
