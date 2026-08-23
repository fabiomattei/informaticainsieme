---
title: 'Permessi dei file in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

I permessi decidono chi può leggere, modificare o eseguire ciascun file. Questa pagina approfondisce il modello dei permessi Linux, dalle basi ai meccanismi più avanzati; la sintassi completa di `chmod` (notazione numerica e simbolica) e di `chown` è già introdotta nella pagina [Utenti, gruppi e permessi]({{ site.baseurl }}{% link _sistemioperativi/linux-utenti.md %}.html).

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

Va usata con attenzione: applicare lo stesso permesso `755` in modo ricorsivo assegna il bit di esecuzione anche ai file "normali" che non dovrebbero averlo (un file di testo non ha bisogno di `x`). Per gestire file e cartelle in modo differenziato in un solo comando si usa `find` insieme a `-exec`:

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
