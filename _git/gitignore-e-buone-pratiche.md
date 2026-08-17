---
title: 'Il file .gitignore e qualche buona pratica'
date: '2026-08-17T10:45:00+02:00'
author: Fabio Mattei
layout: page
---

![Una cartella di progetto con file tracciati e file esclusi dal .gitignore](/images/git/gitignore-e-buone-pratiche/gitignore-e-buone-pratiche.svg){:class="aside-image"}

## Il file .gitignore

Non tutti i file presenti in una cartella di progetto vanno tracciati da Git. Sono da escludere, ad esempio:

* file generati automaticamente (compilati, cache, build);
* dipendenze scaricabili da un gestore di pacchetti (come la cartella `node_modules` di Node.js, o `venv` di Python);
* file di configurazione dell'editor o del sistema operativo (`.DS_Store`, cartelle di VS Code...);
* file contenenti dati sensibili, come password o chiavi di accesso.

Per dire a Git quali file o cartelle ignorare, si crea nella radice del repository un file chiamato `.gitignore`, con un'occorrenza per riga:

{% highlight text %}
# dipendenze
node_modules/
venv/

# file generati
*.log
dist/
__pycache__/

# file di sistema e dell'editor
.DS_Store
.vscode/
{% endhighlight %}

Alcune regole utili sulla sintassi:

* una riga per pattern; le righe che iniziano con `#` sono commenti;
* `*` è un carattere jolly: `*.log` esclude tutti i file con estensione `.log`;
* una barra finale (`node_modules/`) indica che si sta escludendo una cartella intera;
* i pattern si applicano ricorsivamente in tutte le sottocartelle, salvo diversa indicazione.

Il file `.gitignore` va creato **prima** di aggiungere i file da escludere con `git add`: se un file è già tracciato da Git, aggiungerlo dopo a `.gitignore` non lo farà smettere di essere tracciato (per farlo serve rimuoverlo esplicitamente con `git rm --cached nomefile`).

GitHub mette a disposizione modelli di `.gitignore` già pronti per i linguaggi e i framework più comuni, molto utili come punto di partenza: [github.com/github/gitignore](https://github.com/github/gitignore).

## Buone pratiche per i messaggi di commit

Un buon messaggio di commit è un investimento per il futuro: sarà quello che permetterà, a distanza di mesi, di capire perché una certa modifica è stata fatta.

* Scrivere messaggi **brevi ma descrittivi**, che spieghino cosa cambia e, se non ovvio, perché.
* Usare il modo **imperativo**: "Aggiungi il controllo sull'email" piuttosto che "Aggiunto controllo email" o "Aggiungendo il controllo email".
* Fare **commit piccoli e frequenti**, ciascuno dedicato a una singola modifica logica, invece di un unico commit enorme con dentro dieci cose diverse.
* Evitare messaggi generici come `"fix"`, `"aggiornamento"` o `"modifiche varie"`, che non aiutano a capire cosa sia successo.

![Esempi di messaggi di commit da evitare e messaggi scritti meglio](/images/git/gitignore-e-buone-pratiche/commit-messages.svg){:class="half-image"}

## Un primo commit tipico

Mettendo insieme quanto visto finora, l'inizio tipico di un nuovo progetto è:

{% highlight shell %}
git init
# si crea qui il file .gitignore, prima di aggiungere i file
git add .
git commit -m "Commit iniziale"
git branch -M main
git remote add origin https://github.com/utente/nome-progetto.git
git push -u origin main
{% endhighlight %}

`git branch -M main` rinomina forzatamente il branch corrente in `main`: utile perché, a seconda della versione e della configurazione di Git, un repository appena creato con `git init` può chiamare il branch principale `master` invece di `main`, che è oggi la convenzione più diffusa (anche su GitHub).
