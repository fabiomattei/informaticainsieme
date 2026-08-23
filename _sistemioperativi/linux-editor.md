---
title: 'Editor di testo da terminale: nano e vim'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Molte operazioni su Linux (modificare un file di configurazione, scrivere uno script) richiedono di editare testo direttamente da terminale, senza un'interfaccia grafica. I due editor più diffusi sono **nano** e **vim**.

### nano: semplice e immediato

`nano` è pensato per essere intuitivo fin da subito: le scorciatoie disponibili sono mostrate in fondo allo schermo (il simbolo `^` indica `Ctrl`).

{% highlight shell %}
nano appunti.txt
{% endhighlight %}

Le scorciatoie principali:

* **`Ctrl+O`** — salva il file (*Write Out*).
* **`Ctrl+X`** — esce da nano (chiede se salvare le modifiche non ancora salvate).
* **`Ctrl+K`** — taglia la riga corrente.
* **`Ctrl+U`** — incolla il testo tagliato.
* **`Ctrl+W`** — cerca del testo nel file.

Per chi inizia da terminale, `nano` è generalmente lo strumento consigliato.

### vim: più potente, meno immediato

`vim` è uno degli editor più diffusi in ambito professionale, ma ha una curva di apprendimento più ripida perché funziona a **modalità**:

* **Modalità normale** — quella in cui si apre il file: i tasti eseguono comandi, non scrivono testo.
* **Modalità inserimento** — quella in cui si scrive effettivamente testo, attivata con `i`.
* **Modalità comando** — per salvare, uscire e altre operazioni, attivata con `:`.

{% highlight shell %}
vim appunti.txt
{% endhighlight %}

Un primo utilizzo essenziale:

{% highlight shell %}
i            # entra in modalità inserimento per scrivere
Esc          # torna in modalità normale
:w           # salva il file
:q           # esce (fallisce se ci sono modifiche non salvate)
:wq          # salva ed esce
:q!          # esce senza salvare, scartando le modifiche
{% endhighlight %}

Spostamenti utili in modalità normale:

{% highlight shell %}
h j k l      # sposta il cursore a sinistra, giù, su, destra
dd           # elimina la riga corrente
/parola      # cerca "parola" nel file
{% endhighlight %}

### nano in dettaglio: ricerca e sostituzione

Oltre alle scorciatoie di base, nano permette di cercare e sostituire testo:

* **`Ctrl+W`** — cerca una stringa; premendo di nuovo `Ctrl+W` si passa all'occorrenza successiva.
* **`Ctrl+\`** — cerca e sostituisce, chiedendo conferma per ogni occorrenza trovata (o tutte insieme rispondendo `A`, *all*).
* **`Ctrl+_`** — salta direttamente a un numero di riga specifico.
* **`Alt+U`** — annulla l'ultima modifica; **`Alt+E`** la ripristina.

Alcune opzioni utili da riga di comando:

{% highlight shell %}
nano -l script.sh    # mostra i numeri di riga, comodo per gli script
nano -c note.txt      # mostra sempre la posizione del cursore in basso
{% endhighlight %}

### vim in dettaglio: muoversi e modificare più velocemente

Oltre agli spostamenti base (`h j k l`), vim offre comandi più efficienti per muoversi nel testo in modalità normale:

{% highlight shell %}
w            # sposta il cursore all'inizio della parola successiva
b            # sposta il cursore all'inizio della parola precedente
0            # sposta il cursore all'inizio della riga
$            # sposta il cursore alla fine della riga
gg           # va all'inizio del file
G            # va alla fine del file
:42          # va alla riga numero 42
{% endhighlight %}

Comandi di modifica più avanzati:

{% highlight shell %}
x            # elimina il carattere sotto il cursore
yy           # copia (yank) la riga corrente
p            # incolla dopo la riga corrente
u            # annulla l'ultima modifica
Ctrl+r       # ripristina la modifica annullata
{% endhighlight %}

La ricerca e sostituzione in modalità comando usa una sintassi simile a quella dello strumento `sed`:

{% highlight shell %}
:%s/vecchio/nuovo/g      # sostituisce "vecchio" con "nuovo" in tutto il file
:s/vecchio/nuovo/         # sostituisce solo nella riga corrente
{% endhighlight %}

### Personalizzare vim: .vimrc

Come bash legge `~/.bashrc` a ogni avvio, vim legge il file **`~/.vimrc`** per applicare impostazioni personalizzate:

{% highlight shell %}
set number          " mostra i numeri di riga
set autoindent       " mantiene l'indentazione della riga precedente
syntax on            " attiva l'evidenziazione della sintassi
{% endhighlight %}

Le righe che iniziano con `"` sono commenti, ignorati da vim.

### Un'alternativa più recente: VS Code da terminale

Molti sviluppatori, pur lavorando spesso da terminale, preferiscono per l'editing di file più corposi un editor con interfaccia grafica come **Visual Studio Code**. Se installato, si può aprire un file (o un'intera cartella) direttamente dal terminale:

{% highlight shell %}
code appunti.txt      # apre il file in VS Code
code .                 # apre la cartella corrente come progetto
{% endhighlight %}

Questo comporta una dipendenza da un ambiente grafico, cosa che nano e vim non richiedono: su un server senza interfaccia grafica (la situazione più comune quando ci si collega via SSH) restano l'unica opzione praticabile.

### Quale scegliere

Per modifiche rapide e occasionali, `nano` è la scelta più pratica. `vim` conviene impararlo con calma se si prevede di lavorare spesso da terminale: una volta acquisita familiarità con le modalità, permette di editare testo molto più velocemente, senza mai staccare le mani dalla tastiera. Su un server remoto raggiunto via SSH, dove un'interfaccia grafica non è disponibile, la scelta è comunque limitata a editor testuali come questi due.
