---
title: 'Utenti, gruppi e permessi in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Linux è un sistema operativo nato per essere **multiutente**: più persone possono usare lo stesso computer, ciascuna con i propri file e permessi separati.

### L'utente root

L'utente **root** è l'amministratore del sistema, con accesso completo a ogni file e comando. Per motivi di sicurezza non si lavora abitualmente come root: si usa un utente normale ed si eleva temporaneamente il privilegio con `sudo` quando serve (vedi [Il terminale]({{ site.baseurl }}{% link _sistemioperativi/linux-terminale.md %}.html)).

### Creare e gestire utenti

{% highlight shell %}
sudo useradd -m marco       # crea l'utente "marco" con la sua cartella home
sudo passwd marco            # imposta la password per l'utente
sudo userdel -r marco        # elimina l'utente e la sua cartella home
{% endhighlight %}

Per vedere con quale utente si è collegati e a quali gruppi si appartiene:

{% highlight shell %}
whoami          # mostra l'utente corrente
groups          # mostra i gruppi a cui appartiene l'utente corrente
{% endhighlight %}

### I gruppi

Un **gruppo** raccoglie più utenti a cui assegnare gli stessi permessi in blocco, invece di configurarli uno per uno:

{% highlight shell %}
sudo groupadd sviluppatori           # crea un nuovo gruppo
sudo usermod -aG sviluppatori marco  # aggiunge l'utente "marco" al gruppo
{% endhighlight %}

### I permessi in dettaglio

Ogni file ha tre categorie di permessi — lettura (`r`), scrittura (`w`), esecuzione (`x`) — assegnate separatamente a tre soggetti: il **proprietario** (*user*), il **gruppo** proprietario e **tutti gli altri**:

{% highlight shell %}
ls -l appunti.txt
# -rw-r--r-- 1 fabio fabio 1024 ago 23 10:00 appunti.txt
{% endhighlight %}

I dieci caratteri iniziali si leggono così: il primo indica il tipo (`-` file, `d` cartella); i successivi nove sono raggruppati a tre a tre per proprietario, gruppo e altri (`rw-` `r--` `r--` nell'esempio).

### chmod: modificare i permessi

I permessi si possono esprimere in forma numerica, sommando i valori di ciascun permesso (lettura = 4, scrittura = 2, esecuzione = 1):

{% highlight shell %}
chmod 644 appunti.txt   # proprietario: rw- (6)  gruppo: r-- (4)  altri: r-- (4)
chmod 755 script.sh      # proprietario: rwx (7)  gruppo: r-x (5)  altri: r-x (5)
{% endhighlight %}

In alternativa, si possono modificare i permessi in forma simbolica:

{% highlight shell %}
chmod u+x script.sh     # aggiunge il permesso di esecuzione al proprietario (u = user)
chmod g-w appunti.txt    # toglie il permesso di scrittura al gruppo (g = group)
chmod o=r appunti.txt    # imposta i permessi degli altri (o = other) a sola lettura
{% endhighlight %}

### chown: cambiare proprietario

{% highlight shell %}
sudo chown marco appunti.txt            # cambia il proprietario del file
sudo chown marco:sviluppatori appunti.txt  # cambia proprietario e gruppo insieme
{% endhighlight %}

### Dove sono registrati gli utenti: /etc/passwd

L'elenco di tutti gli utenti del sistema è registrato nel file **`/etc/passwd`**, leggibile da chiunque:

{% highlight shell %}
cat /etc/passwd
{% endhighlight %}

Ogni riga descrive un utente con campi separati da `:` — nome utente, un segnaposto per la password (`x`), UID (identificativo numerico dell'utente), GID (identificativo del gruppo principale), descrizione, cartella home e shell predefinita:

{% highlight shell %}
fabio:x:1000:1000:Fabio Mattei:/home/fabio:/bin/bash
{% endhighlight %}

Nonostante il nome, le password vere non sono più salvate qui da decenni: il campo contiene solo `x` come segnaposto.

### Dove sono le password: /etc/shadow

Le password, cifrate, sono conservate nel file **`/etc/shadow`**, leggibile solo da root:

{% highlight shell %}
sudo cat /etc/shadow
{% endhighlight %}

Questa separazione tra `/etc/passwd` (leggibile da tutti, necessario ad esempio perché i comandi possano tradurre UID in nomi utente) e `/etc/shadow` (riservato) è una misura di sicurezza: anche un utente senza privilegi può leggere l'elenco degli utenti, ma non le password cifrate.

### su e sudo: due modi di elevare i privilegi

* **`sudo comando`** — esegue un singolo comando con privilegi di root, chiedendo la password dell'utente corrente (non quella di root). È il metodo consigliato: ogni utilizzo resta tracciato nei log di sistema.
* **`su`** — apre una sessione shell come un altro utente (root per default), chiedendo la password *di quell'utente*:

{% highlight shell %}
su -            # apre una shell come root, chiedendo la password di root
su marco         # apre una shell come l'utente "marco"
{% endhighlight %}

Su molte distribuzioni moderne (come Ubuntu) l'account root non ha nemmeno una password impostata di default, proprio per incoraggiare l'uso di `sudo` al posto di `su`.

### Il file sudoers

Quali utenti possono usare `sudo`, e per quali comandi, è configurato nel file **`/etc/sudoers`**. Non va mai modificato direttamente con un editor qualsiasi, ma con il comando dedicato, che verifica la sintassi prima di salvare (un errore in questo file può rendere `sudo` inutilizzabile per tutti):

{% highlight shell %}
sudo visudo
{% endhighlight %}

Una riga tipica concede a un utente pieni permessi sudo:

{% highlight shell %}
marco   ALL=(ALL:ALL) ALL
{% endhighlight %}

In pratica, gran parte delle distribuzioni desktop gestisce questo tramite l'appartenenza a un gruppo predefinito (`sudo` su Debian/Ubuntu, `wheel` su Fedora/Arch): aggiungere un utente a quel gruppo con `usermod -aG` (visto sopra) equivale a concedergli i permessi sudo, senza dover editare `/etc/sudoers` a mano.

### UID e permessi: il vero significato del proprietario di un file

Quando si esegue `ls -l` e si vede il nome di un utente come proprietario di un file, quel nome è in realtà una traduzione leggibile dell'**UID** memorizzato nel file system. Se un utente viene eliminato e il suo UID riassegnato a un altro utente, i vecchi file mostreranno come proprietario il nuovo utente: il sistema, sotto il cofano, ragiona sempre per numeri, non per nomi.
