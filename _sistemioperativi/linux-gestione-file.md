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

### Permessi

Ogni file in Linux ha permessi di lettura, scrittura ed esecuzione, assegnati separatamente a proprietario, gruppo e altri utenti:

{% highlight shell %}
ls -l appunti.txt
# -rw-r--r-- 1 fabio fabio 1024 ago 23 10:00 appunti.txt
{% endhighlight %}

I permessi si modificano con `chmod` e il proprietario con `chown`:

{% highlight shell %}
chmod 644 appunti.txt   # lettura/scrittura per il proprietario, sola lettura per gli altri
chown fabio appunti.txt # cambia il proprietario del file
{% endhighlight %}

La sintassi completa di `chmod` (notazione numerica e simbolica) e di `chown` è approfondita nella pagina [Utenti, gruppi e permessi]({{ site.baseurl }}{% link _sistemioperativi/linux-utenti.md %}.html). Qui vediamo alcuni aspetti pratici che riguardano più da vicino il lavoro quotidiano con i file.

### Cosa significa "esecuzione" su una cartella

Il permesso di esecuzione (`x`) ha un significato diverso a seconda che si applichi a un file o a una cartella:

* Su un **file**, `x` permette di eseguirlo come un programma o uno script.
* Su una **cartella**, `x` non ha nulla a che fare con l'"esecuzione": permette di **entrarci** (con `cd`) e di accedere ai file al suo interno conoscendone il nome. Senza `x`, anche avendo il permesso di lettura (`r`) su una cartella, non è possibile aprirla con `cd` né accedere al contenuto dei file che contiene.

Il permesso di lettura (`r`) su una cartella, da solo, permette solo di **elencarne il contenuto** (`ls`), non di entrarci. Per questo le cartelle hanno quasi sempre sia `r` sia `x` insieme: l'una senza l'altra è raramente utile.

### Applicare permessi e proprietario in modo ricorsivo

Per modificare permessi o proprietario non solo di una cartella, ma anche di tutto il suo contenuto, si usa l'opzione `-R` (*recursive*):

{% highlight shell %}
chmod -R 755 progetti/          # applica i permessi a "progetti" e a tutto ciò che contiene
sudo chown -R fabio:fabio progetti/  # cambia proprietario e gruppo ricorsivamente
{% endhighlight %}

Va usata con attenzione: applicare lo stesso permesso `755` in modo ricorsivo assegna il bit di esecuzione anche ai file "normali" che non dovrebbero averlo (un file di testo non ha bisogno di `x`). Per gestire file e cartelle in modo differenziato in un solo comando si usa `find` insieme a `-exec`, già visto in precedenza:

{% highlight shell %}
find progetti/ -type d -exec chmod 755 {} \;   # 755 solo alle cartelle
find progetti/ -type f -exec chmod 644 {} \;   # 644 solo ai file
{% endhighlight %}

### Oltre proprietario/gruppo/altri: le ACL

Il modello classico (proprietario, gruppo, altri) permette di assegnare permessi a **un solo** utente e **un solo** gruppo per volta. Quando serve dare permessi differenziati a più utenti o gruppi sullo stesso file, senza doverli aggiungere tutti allo stesso gruppo, si usano le **ACL** (Access Control List), un'estensione del file system che permette regole più granulari:

{% highlight shell %}
sudo setfacl -m u:marco:rw appunti.txt   # concede a "marco" permessi di lettura/scrittura, oltre ai permessi normali
getfacl appunti.txt                       # mostra tutte le regole ACL attive su un file
sudo setfacl -x u:marco appunti.txt       # rimuove la regola ACL per "marco"
{% endhighlight %}

Un file con ACL attive mostra un `+` alla fine della stringa dei permessi in `ls -l` (es. `-rw-r--r--+`), a segnalare che esistono regole aggiuntive oltre a quelle visibili nella forma classica.

### Rendere un file immutabile: chattr

Oltre ai permessi tradizionali, il file system ext4 supporta alcuni **attributi** aggiuntivi, indipendenti da chi possiede il file. Il più usato è l'attributo immutabile, che impedisce qualsiasi modifica al file, anche da parte del proprietario o di root:

{% highlight shell %}
sudo chattr +i file_importante.conf   # rende il file immutabile: non può essere modificato né eliminato
lsattr file_importante.conf            # mostra gli attributi attivi su un file
sudo chattr -i file_importante.conf    # rimuove l'attributo immutabile
{% endhighlight %}

Utile per proteggere file di configurazione critici da modifiche accidentali, anche involontarie da parte di root stesso: per rimuovere l'attributo bisogna prima disattivarlo esplicitamente con `chattr -i`.

### Trovare i file

{% highlight shell %}
find . -name "*.txt"           # cerca ricorsivamente i file .txt a partire dalla cartella corrente
find . -type d -name "backup"  # cerca solo cartelle (non file) di nome "backup"
find . -size +10M               # cerca file più grandi di 10 MB
{% endhighlight %}

Per cercare del testo **dentro** i file (non nei loro nomi) si usa `grep`:

{% highlight shell %}
grep "errore" log.txt          # mostra le righe di log.txt che contengono "errore"
grep -r "errore" .              # cerca ricorsivamente in tutti i file della cartella corrente
{% endhighlight %}

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

### Confrontare due file

{% highlight shell %}
diff versione1.txt versione2.txt
{% endhighlight %}

Mostra le righe che differiscono tra i due file, con il formato usato anche dagli strumenti di controllo versione come Git per calcolare le modifiche tra due revisioni.

### Permessi speciali

Oltre a lettura, scrittura ed esecuzione, esistono tre permessi speciali meno comuni ma importanti da conoscere:

* **setuid** — su un file eseguibile, fa sì che il programma venga eseguito con i permessi del suo proprietario invece che di chi lo lancia. È il meccanismo, ad esempio, che permette a `passwd` di modificare il file delle password anche se lanciato da un utente normale.
* **setgid** — simile a setuid, ma riferito al gruppo; su una cartella, fa sì che i nuovi file creati al suo interno ereditino automaticamente il gruppo della cartella invece di quello dell'utente che li crea.
* **sticky bit** — su una cartella condivisa da più utenti (come `/tmp`), impedisce che un utente possa eliminare o rinominare file di cui non è proprietario, anche se ha permesso di scrittura sulla cartella.

Si impostano con `chmod` usando una cifra aggiuntiva prima delle tre standard:

{% highlight shell %}
chmod 4755 programma    # imposta setuid (4) oltre ai permessi rwxr-xr-x
chmod 1777 /tmp          # imposta lo sticky bit (1), tipico delle cartelle condivise
{% endhighlight %}

### umask: i permessi predefiniti

Quando si crea un nuovo file o una nuova cartella, i permessi assegnati automaticamente dipendono dalla **umask**, un valore che "sottrae" permessi da quelli massimi predefiniti (666 per i file, 777 per le cartelle):

{% highlight shell %}
umask         # mostra la umask corrente (tipicamente 022)
{% endhighlight %}

Con una umask di `022`, un nuovo file viene creato con permessi `644` (666 - 022) e una nuova cartella con permessi `755` (777 - 022).

### Ricerca avanzata con find

Il comando `find`, già visto in forma base, accetta molte altre condizioni combinabili tra loro:

{% highlight shell %}
find . -name "*.txt" -mtime -7        # file .txt modificati negli ultimi 7 giorni
find . -name "*.log" -delete           # trova ed elimina direttamente i file trovati
find . -type f -exec chmod 644 {} \;   # esegue un comando su ogni file trovato
{% endhighlight %}

L'opzione `-exec` esegue il comando indicato su ciascun risultato: `{}` viene sostituito dal nome del file trovato, e la sequenza termina con `\;`.
