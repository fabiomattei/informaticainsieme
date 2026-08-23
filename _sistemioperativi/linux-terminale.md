---
title: 'Il terminale in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Il **terminale** è lo strumento con cui su Linux si impartiscono comandi testuali al sistema operativo. È il punto di partenza per capire come funzionano davvero la gestione dei file e della rete, argomenti approfonditi nelle pagine successive.

### Terminale, shell e prompt

Questi tre termini vengono spesso usati come sinonimi, ma indicano cose diverse:

* **Terminale** (o *terminale emulato*) — la finestra o l'applicazione che permette di digitare comandi e leggerne l'output.
* **Shell** — il programma che interpreta i comandi digitati e li esegue. La più diffusa su Linux è **bash** (Bourne Again SHell); altre shell comuni sono `zsh` e `fish`.
* **Prompt** — la riga su cui si digita, che termina di solito con `$` per un utente normale e `#` per l'utente root (amministratore):

{% highlight shell %}
fabio@computer:~$
{% endhighlight %}

### La struttura di un comando

Un comando è generalmente composto da tre parti:

{% highlight shell %}
comando -opzione argomento
{% endhighlight %}

Ad esempio:

{% highlight shell %}
ls -l /home
{% endhighlight %}

Qui `ls` è il comando, `-l` è un'opzione che ne modifica il comportamento (formato con più dettagli) e `/home` è l'argomento su cui il comando agisce. Le opzioni si scrivono con un trattino singolo se abbreviate (`-l`) o doppio se per esteso (`--list`), e possono spesso essere combinate: `ls -la` equivale a `ls -l -a`.

### Autocompletamento e cronologia

* **Tab** — completa automaticamente nomi di comandi, file e cartelle; premuto due volte mostra tutte le possibilità quando ce n'è più di una.
* **Freccia su/giù** — scorre i comandi digitati in precedenza.
* **`Ctrl+R`** — avvia una ricerca nella cronologia dei comandi già eseguiti.

### Redirezione e pipe

L'output di un comando può essere reindirizzato invece di essere mostrato a schermo:

{% highlight shell %}
ls -l > elenco.txt     # salva l'output in un file (sovrascrivendolo)
ls -l >> elenco.txt    # aggiunge l'output in coda al file
{% endhighlight %}

Il simbolo **pipe** (`|`) collega l'output di un comando all'input del successivo, permettendo di combinare più comandi:

{% highlight shell %}
ls -l | grep ".txt"    # mostra solo le righe dell'elenco che contengono ".txt"
{% endhighlight %}

### Permessi di amministratore: sudo

Alcune operazioni (installare programmi, modificare file di sistema) richiedono privilegi di amministratore. Si ottengono anteponendo `sudo` (*superuser do*) al comando:

{% highlight shell %}
sudo apt update
{% endhighlight %}

Il sistema chiederà la password dell'utente corrente (non quella di root) prima di eseguire il comando con privilegi elevati.

### Ottenere aiuto

* **`comando --help`** — mostra una guida rapida alle opzioni del comando.
* **`man comando`** — apre il manuale completo del comando (per uscire: `q`).

{% highlight shell %}
man ls
{% endhighlight %}

### Alcune scorciatoie utili

* **`Ctrl+C`** — interrompe il comando in esecuzione.
* **`Ctrl+D`** — chiude il terminale o la sessione corrente.
* **`Ctrl+L`** (o il comando `clear`) — pulisce lo schermo dal testo scritto finora.

### Variabili d'ambiente

Una **variabile d'ambiente** è un valore che la shell rende disponibile ai programmi che avvia. Per vederne una:

{% highlight shell %}
echo $HOME     # mostra il valore della variabile HOME (la cartella personale dell'utente)
echo $PATH     # mostra l'elenco delle cartelle in cui la shell cerca i comandi da eseguire
{% endhighlight %}

`PATH` è la variabile più importante: quando si digita un comando come `ls`, la shell lo cerca in ognuna delle cartelle elencate in `PATH`, nell'ordine, finché non lo trova. È il motivo per cui basta scrivere `ls` invece di `/bin/ls`.

Per impostare una variabile nella sessione corrente:

{% highlight shell %}
export EDITOR=nano
{% endhighlight %}

### Alias

Un **alias** è un nome breve che sostituisce un comando più lungo:

{% highlight shell %}
alias ll='ls -la'
{% endhighlight %}

Da quel momento, digitando `ll` verrà eseguito `ls -la`. Un alias definito così vale solo per la sessione corrente del terminale.

### Personalizzare la shell: .bashrc

Ogni volta che si apre un nuovo terminale, bash esegue automaticamente il file **`~/.bashrc`** (nella cartella personale dell'utente). È il posto giusto in cui inserire variabili d'ambiente, alias e altre personalizzazioni che si vogliono avere disponibili in ogni sessione, senza doverle ridigitare ogni volta:

{% highlight shell %}
nano ~/.bashrc
{% endhighlight %}

Dopo aver modificato il file, per applicare le modifiche alla sessione già aperta (senza doverla chiudere e riaprire):

{% highlight shell %}
source ~/.bashrc
{% endhighlight %}

### Espansione dei caratteri jolly (glob)

La shell stessa, prima ancora di eseguire un comando, sostituisce alcuni simboli speciali con l'elenco dei file che corrispondono. Questo meccanismo si chiama **glob** (o *wildcard*):

{% highlight shell %}
ls *.txt          # tutti i file con estensione .txt
ls foto??.jpg      # foto seguito da esattamente due caratteri, poi .jpg
ls file[123].txt   # file1.txt, file2.txt o file3.txt
{% endhighlight %}

* `*` corrisponde a una sequenza qualsiasi di caratteri (anche vuota).
* `?` corrisponde a un singolo carattere.
* `[...]` corrisponde a uno dei caratteri elencati tra parentesi quadre.

È importante ricordare che è la shell a espandere questi simboli **prima** di eseguire il comando: `ls *.txt` diventa, ad esempio, `ls appunti.txt note.txt`. Il comando `ls` non sa nulla dei caratteri jolly, riceve semplicemente un elenco di nomi già espansi.

### Sostituzione di comandi e sotto-shell

Oltre a `$( )` (già visto per catturare l'output di un comando), la shell permette di raggruppare più comandi in una **sotto-shell** tramite le parentesi tonde, eseguendoli in un ambiente separato da quello corrente:

{% highlight shell %}
(cd /tmp && ls)    # cambia cartella solo all'interno della sotto-shell
pwd                 # la cartella corrente della shell principale non è cambiata
{% endhighlight %}

Le parentesi graffe `{ }`, invece, eseguono i comandi nella shell corrente, senza crearne una nuova.

### Concatenare comandi in sequenza

Oltre alla pipe, la shell offre altri operatori per collegare comandi diversi:

{% highlight shell %}
mkdir progetti && cd progetti   # esegue "cd progetti" solo se "mkdir" ha avuto successo
comando_inesistente || echo "errore"  # esegue "echo" solo se il comando precedente fallisce
comando1; comando2                # esegue comando2 dopo comando1, indipendentemente dall'esito
{% endhighlight %}

`&&` esegue il comando successivo solo se il precedente è terminato con successo (codice di uscita 0); `||` solo se è fallito; `;` esegue comunque, in sequenza, senza condizioni.

### Job control avanzato

Oltre a `jobs`, `fg` e `bg` (già visti per i processi in background), ogni job ha un numero identificativo che si può usare per riferirsi a un processo specifico quando ce n'è più di uno in background:

{% highlight shell %}
jobs -l          # elenca i job in background con il rispettivo PID
fg %2             # riporta in primo piano il job numero 2
kill %1           # termina il job numero 1
{% endhighlight %}

Un processo avviato con `nohup` continua a girare anche se il terminale da cui è stato lanciato viene chiuso, utile per processi lunghi avviati su una connessione SSH che potrebbe interrompersi:

{% highlight shell %}
nohup ./backup.sh &
{% endhighlight %}

### Personalizzare il prompt

L'aspetto del prompt (quello che prima abbiamo visto nella forma `fabio@computer:~$`) è controllato dalla variabile `PS1`, che si può personalizzare in `~/.bashrc`:

{% highlight shell %}
PS1='\u@\h:\w\$ '
{% endhighlight %}

Alcuni codici comuni: `\u` il nome utente, `\h` il nome del computer, `\w` la cartella corrente, `\$` il simbolo `$` (o `#` per root). Molte distribuzioni aggiungono anche i colori tramite sequenze di escape, per rendere il prompt più leggibile a colpo d'occhio.

### File nascosti e file di configurazione dell'utente

I file e le cartelle il cui nome inizia con un punto (`.bashrc`, `.ssh`, `.config`) sono considerati **nascosti**: non compaiono con `ls` semplice, ma solo con `ls -a` o `ls -la`. Per convenzione, contengono le impostazioni personali dell'utente, separate dai file di configurazione di sistema in `/etc`.
