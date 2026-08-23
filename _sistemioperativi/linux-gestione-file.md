---
title: 'Gestione dei file in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Su Linux la gestione dei file avviene tipicamente da **terminale**, usando la shell (in genere `bash`). Rispetto a Windows non esistono lettere di unità: tutto il file system parte da un'unica radice.

### Il file system: una gerarchia unica

Linux organizza tutto sotto un'unica radice, indicata con `/` (*root*). Non esistono unità separate come `C:` o `D:`: dischi e periferiche aggiuntive vengono "montati" (*mount*) all'interno di questa gerarchia, in punti come `/mnt` o `/media`. Alcune cartelle standard:

* **`/home/nomeutente`** — la cartella personale di ogni utente, equivalente a `C:\Users\NomeUtente` su Windows.
* **`/etc`** — i file di configurazione del sistema.
* **`/bin`** e **`/usr/bin`** — i programmi eseguibili di base.
* **`/var`** — dati variabili, come i file di log.

### Navigare tra le cartelle

{% highlight shell %}
pwd          # mostra la cartella corrente
ls -la       # elenca il contenuto della cartella, inclusi i file nascosti
cd progetti  # entra nella cartella "progetti"
cd ..        # risale alla cartella superiore
{% endhighlight %}

### Creare, copiare, spostare ed eliminare

{% highlight shell %}
mkdir progetti                  # crea una cartella
cp appunti.txt backup/          # copia un file
cp -r progetti backup/          # copia una cartella e il suo contenuto
mv appunti.txt note.txt         # rinomina (o sposta) un file
rm appunti.txt                  # elimina un file
rm -r progetti                  # elimina una cartella e il suo contenuto
{% endhighlight %}

A differenza di Windows, `rm` elimina i file **definitivamente**: non esiste un Cestino di sistema per le operazioni da terminale.

I permessi di file e cartelle sono approfonditi nella pagina dedicata [Permessi dei file in Linux]({{ site.baseurl }}{% link _sistemioperativi/linux-permessi.md %}.html).

### Trovare i file

{% highlight shell %}
find . -name "*.txt"           # cerca ricorsivamente i file .txt a partire dalla cartella corrente
find . -type d -name "backup"  # cerca solo cartelle (non file) di nome "backup"
find . -size +10M               # cerca file più grandi di 10 MB
{% endhighlight %}

Per cercare del testo **dentro** i file (non nei loro nomi) si usa `grep`, approfondito nella pagina [Elaborazione del testo da terminale]({{ site.baseurl }}{% link _sistemioperativi/linux-elaborazione-testo.md %}.html) insieme ad altri strumenti come `sed` e `awk`.

### Ricerca avanzata con find

Il comando `find`, già visto in forma base, accetta molte altre condizioni combinabili tra loro:

{% highlight shell %}
find . -name "*.txt" -mtime -7        # file .txt modificati negli ultimi 7 giorni
find . -name "*.log" -delete           # trova ed elimina direttamente i file trovati
find . -type f -exec chmod 644 {} \;   # esegue un comando su ogni file trovato
{% endhighlight %}

L'opzione `-exec` esegue il comando indicato su ciascun risultato: `{}` viene sostituito dal nome del file trovato, e la sequenza termina con `\;`.

### Link simbolici

Un **link simbolico** (o *symlink*) è un file speciale che punta a un altro file o cartella, in modo simile ai collegamenti di Windows:

{% highlight shell %}
ln -s /percorso/originale collegamento
{% endhighlight %}

Aprendo `collegamento` si accede in realtà al file originale. È utile ad esempio per rendere disponibile uno stesso file in più punti del file system senza doverlo duplicare.

### Spazio occupato su disco

{% highlight shell %}
df -h            # mostra lo spazio libero e occupato su ciascuna unità montata
du -sh cartella  # mostra lo spazio totale occupato da una cartella
{% endhighlight %}

L'opzione `-h` (*human-readable*) mostra le dimensioni in KB/MB/GB invece che in byte.

### Archiviare e comprimere

Il formato più diffuso su Linux per archiviare più file in uno solo è **tar**, spesso combinato con la compressione **gzip**:

{% highlight shell %}
tar -czvf archivio.tar.gz progetti/    # crea un archivio compresso della cartella "progetti"
tar -xzvf archivio.tar.gz               # estrae il contenuto dell'archivio
{% endhighlight %}

Le opzioni si leggono così: `c` crea, `x` estrae, `z` comprime/decomprime con gzip, `v` mostra i file elaborati (*verbose*), `f` indica che segue il nome del file di archivio.

### Leggere il contenuto di un file senza aprirlo in un editor

{% highlight shell %}
cat appunti.txt         # mostra tutto il contenuto del file
less appunti.txt         # mostra il contenuto una schermata alla volta (frecce per scorrere, "q" per uscire)
head appunti.txt          # mostra le prime 10 righe
head -n 5 appunti.txt      # mostra le prime 5 righe
tail appunti.txt           # mostra le ultime 10 righe
tail -f log.txt             # mostra le ultime righe e resta in ascolto di nuove righe aggiunte
{% endhighlight %}

`tail -f` è particolarmente utile per seguire in tempo reale un file di log che si sta aggiornando, ad esempio quello di un servizio in esecuzione.

### cat in dettaglio

`cat` (*concatenate*) prende il nome dalla sua funzione originaria: unire più file in uno solo, semplicemente mostrandoli in sequenza:

{% highlight shell %}
cat appunti.txt                     # mostra il contenuto di un file
cat capitolo1.txt capitolo2.txt     # mostra i due file uno dopo l'altro
cat capitolo1.txt capitolo2.txt > libro.txt   # unisce i due file salvando il risultato in un nuovo file
cat -n appunti.txt                   # mostra il contenuto numerando ogni riga
{% endhighlight %}

Per file lunghi, mostrare tutto lo schermo in una volta è scomodo: in quel caso conviene `less` (visto sopra), che permette di scorrere e cercare senza far scorrere via l'intero contenuto dal terminale. `cat` resta la scelta più immediata per file brevi o quando l'output serve come input per un altro comando tramite una pipe:

{% highlight shell %}
cat log.txt | grep "errore"
{% endhighlight %}

### Confrontare due file

{% highlight shell %}
diff versione1.txt versione2.txt
{% endhighlight %}

Mostra le righe che differiscono tra i due file, con il formato usato anche dagli strumenti di controllo versione come Git per calcolare le modifiche tra due revisioni.
