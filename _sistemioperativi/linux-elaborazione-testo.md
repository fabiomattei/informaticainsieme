---
title: 'Elaborazione del testo da terminale in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Linux offre una serie di piccoli strumenti da riga di comando, ciascuno specializzato in un compito preciso, pensati per essere combinati tra loro tramite le pipe (già viste nella pagina [Il terminale]({{ site.baseurl }}{% link _sistemioperativi/linux-terminale.md %}.html)). Questa pagina raccoglie i più usati per cercare, trasformare e analizzare testo.

### grep: cercare testo dentro i file

{% highlight shell %}
grep "errore" log.txt          # mostra le righe di log.txt che contengono "errore"
grep -r "errore" .              # cerca ricorsivamente in tutti i file della cartella corrente
{% endhighlight %}

### grep in dettaglio

Alcune opzioni rendono `grep` molto più utile nell'uso quotidiano:

{% highlight shell %}
grep -i "errore" log.txt        # ignora maiuscole/minuscole (case-insensitive)
grep -v "debug" log.txt          # mostra le righe che NON contengono "debug" (inverte la ricerca)
grep -c "errore" log.txt         # conta le righe corrispondenti, invece di mostrarle
grep -n "errore" log.txt         # mostra anche il numero di riga di ogni corrispondenza
grep -w "log" appunti.txt         # cerca "log" come parola intera, non come sottostringa di altre parole
{% endhighlight %}

`grep` supporta anche le **espressioni regolari**, un modo per descrivere pattern di testo più complessi di una semplice sottostringa:

{% highlight shell %}
grep -E "^[0-9]+" numeri.txt     # righe che iniziano con una sequenza di cifre
grep -E "errore|avviso" log.txt   # righe che contengono "errore" oppure "avviso"
{% endhighlight %}

L'opzione `-E` attiva le espressioni regolari estese; `^` indica l'inizio riga, `[0-9]+` una o più cifre, `|` funziona come un "oppure" tra alternative.

Quando un risultato serve capirlo nel contesto delle righe vicine, `grep` può mostrare anche le righe intorno a quella trovata:

{% highlight shell %}
grep -A 3 "errore" log.txt      # mostra la riga trovata più le 3 righe successive (After)
grep -B 3 "errore" log.txt       # mostra la riga trovata più le 3 righe precedenti (Before)
grep -C 3 "errore" log.txt        # mostra 3 righe di contesto sia prima sia dopo (Context)
{% endhighlight %}

Utile in fase di debug, quando l'errore da solo non basta a capire cosa lo ha causato. Altre due opzioni frequenti:

{% highlight shell %}
grep -o -E "[0-9]+" dati.txt      # mostra solo la parte di riga che corrisponde al pattern, non l'intera riga
grep -l "errore" *.log             # elenca solo i nomi dei file che contengono almeno una corrispondenza
{% endhighlight %}

Quando si cerca in più file contemporaneamente, `grep` antepone automaticamente il nome del file a ogni riga trovata, per distinguere da dove proviene ciascun risultato:

{% highlight shell %}
grep "errore" *.log
# app.log:12:errore di connessione
# sistema.log:8:errore critico
{% endhighlight %}

### sed: modificare testo da riga di comando

`sed` (*stream editor*) applica trasformazioni al testo che riceve, riga per riga, senza aprire alcun editor. L'uso più comune è la sostituzione:

{% highlight shell %}
sed 's/vecchio/nuovo/' appunti.txt        # sostituisce la prima occorrenza per riga e stampa il risultato
sed 's/vecchio/nuovo/g' appunti.txt        # sostituisce tutte le occorrenze in ogni riga ("g" = global)
{% endhighlight %}

Per impostazione predefinita, `sed` non modifica il file: stampa a schermo il risultato della trasformazione, lasciando il file originale intatto. Per applicare le modifiche direttamente al file si usa l'opzione `-i` (*in-place*):

{% highlight shell %}
sed -i 's/vecchio/nuovo/g' appunti.txt
{% endhighlight %}

Altri usi frequenti di `sed`:

{% highlight shell %}
sed -n '3,5p' appunti.txt      # stampa solo le righe dalla 3 alla 5 (-n sopprime la stampa automatica, "p" stampa)
sed '2d' appunti.txt            # elimina la seconda riga (stampando il resto)
sed '/^#/d' config.txt           # elimina tutte le righe che iniziano con "#" (utile per togliere i commenti)
{% endhighlight %}

`sed` può anche aggiungere testo, non solo modificarlo o eliminarlo:

{% highlight shell %}
sed '3a testo aggiunto' appunti.txt      # inserisce una riga dopo la riga 3 (append)
sed '3i testo inserito' appunti.txt       # inserisce una riga prima della riga 3 (insert)
sed '$a ultima riga' appunti.txt           # aggiunge una riga alla fine del file ("$" indica l'ultima riga)
{% endhighlight %}

Più trasformazioni possono essere applicate nello stesso comando con `-e`, oppure separandole con `;`:

{% highlight shell %}
sed -e 's/errore/ERRORE/g' -e 's/avviso/AVVISO/g' log.txt
sed 's/errore/ERRORE/g; s/avviso/AVVISO/g' log.txt
{% endhighlight %}

Un'ultima funzione utile è la traslitterazione di singoli caratteri con `y` (concettualmente simile a `tr`, visto più avanti, ma limitata a un singolo comando `sed` già in corso):

{% highlight shell %}
sed 'y/abc/xyz/' appunti.txt       # sostituisce ogni "a" con "x", ogni "b" con "y", ogni "c" con "z"
{% endhighlight %}

`grep`, `sed` e le pipe si combinano spesso insieme: un flusso tipico è filtrare le righe interessanti con `grep` e poi trasformarle con `sed`:

{% highlight shell %}
grep "errore" log.txt | sed 's/errore/ERRORE/'
{% endhighlight %}

### awk: elaborare testo per colonne

Mentre `sed` lavora riga per riga sull'intero testo, `awk` è pensato per testo organizzato in **colonne** (campi separati, per default, da spazi o tabulazioni), come l'output di molti comandi Linux. Ogni campo è accessibile con `$1`, `$2`, ecc.; `$0` indica l'intera riga:

{% highlight shell %}
awk '{print $1}' appunti.txt          # stampa solo la prima colonna di ogni riga
awk '{print $1, $3}' appunti.txt       # stampa la prima e la terza colonna
{% endhighlight %}

Un esempio pratico: `ls -l` restituisce le dimensioni dei file nella quinta colonna, il nome del file nell'ultima:

{% highlight shell %}
ls -l | awk '{print $5, $9}'
{% endhighlight %}

`awk` supporta anche condizioni, applicate solo alle righe che le soddisfano:

{% highlight shell %}
awk '$3 > 100 {print $1}' dati.txt    # stampa la prima colonna solo per le righe in cui la terza è > 100
{% endhighlight %}

Con file separati da un carattere diverso dallo spazio (come i CSV, separati da virgola), si usa l'opzione `-F` per indicare il separatore:

{% highlight shell %}
awk -F, '{print $2}' dati.csv          # stampa la seconda colonna di un file CSV
{% endhighlight %}

`awk` ha anche variabili predefinite molto usate: `NR` (*Number of Records*) contiene il numero della riga corrente, `NF` (*Number of Fields*) il numero di colonne trovate in quella riga:

{% highlight shell %}
awk '{print NR, $0}' appunti.txt        # numera ogni riga, come "cat -n"
awk '{print $NF}' dati.txt               # stampa sempre l'ultima colonna, qualunque sia il loro numero
awk 'NR==3' appunti.txt                   # stampa solo la riga numero 3
{% endhighlight %}

Un programma awk può anche avere un blocco `BEGIN` (eseguito prima di leggere qualsiasi riga, utile per intestazioni) e un blocco `END` (eseguito dopo l'ultima riga, utile per totali):

{% highlight shell %}
awk 'BEGIN {print "Elenco file:"} {print $9} END {print "Fine elenco, righe totali:", NR}' <(ls -l)
{% endhighlight %}

Per un output formattato con maggiore controllo (allineamento, decimali) si usa `printf` invece di `print`, con una sintassi simile a quella del linguaggio C:

{% highlight shell %}
awk '{printf "%-10s %5d\n", $1, $2}' dati.txt
{% endhighlight %}

`%-10s` significa "stringa allineata a sinistra su 10 caratteri", `%5d` "numero intero allineato su 5 caratteri": utile per produrre tabelle leggibili direttamente da terminale.

### wc: contare righe, parole e caratteri

{% highlight shell %}
wc appunti.txt        # mostra righe, parole e caratteri
wc -l appunti.txt      # conta solo le righe
wc -w appunti.txt       # conta solo le parole
wc -c appunti.txt        # conta i byte
wc -m appunti.txt         # conta i caratteri (diverso da -c se il file contiene caratteri accentati o non ASCII)
{% endhighlight %}

Passando più file, `wc` mostra un conteggio per ciascuno e un totale finale:

{% highlight shell %}
wc -l capitolo1.txt capitolo2.txt capitolo3.txt
#   120 capitolo1.txt
#    85 capitolo2.txt
#   200 capitolo3.txt
#   405 totale
{% endhighlight %}

Usato spesso in fondo a una pipe, per contare quanti risultati produce un comando precedente:

{% highlight shell %}
grep -c "errore" log.txt      # conteggio diretto con grep -c
grep "errore" log.txt | wc -l   # equivalente, passando l'output a wc
ls | wc -l                       # conta quanti file/cartelle contiene la cartella corrente
{% endhighlight %}

### sort: ordinare righe

{% highlight shell %}
sort nomi.txt              # ordina le righe in ordine alfabetico
sort -r nomi.txt             # ordine alfabetico inverso
sort -n numeri.txt            # ordinamento numerico (senza -n, "10" verrebbe prima di "2")
sort -k2 dati.txt              # ordina in base alla seconda colonna
{% endhighlight %}

Altre opzioni utili:

{% highlight shell %}
sort -u nomi.txt              # ordina ed elimina i duplicati in un solo passaggio (equivalente a "sort | uniq")
sort -h dimensioni.txt          # ordinamento "umano": riconosce 1K, 2M, 3G come valori crescenti
sort -t: -k3 -n /etc/passwd      # usa ":" come separatore e ordina numericamente sulla terza colonna (l'UID)
{% endhighlight %}

Con `-k`, il numero indicato si riferisce alla colonna su cui ordinare, considerando come separatore lo spazio (o il carattere indicato con `-t`).

### uniq: eliminare righe duplicate consecutive

{% highlight shell %}
uniq nomi.txt          # rimuove le righe duplicate, ma solo se consecutive
uniq -c nomi.txt         # conta quante volte si ripete ogni riga
{% endhighlight %}

`uniq` elimina solo i duplicati **adiacenti**: per questo si usa quasi sempre insieme a `sort`, che raggruppa le righe uguali rendendole consecutive:

{% highlight shell %}
sort nomi.txt | uniq            # elenco senza duplicati
sort nomi.txt | uniq -c | sort -rn   # frequenza di ogni riga, dalla più comune alla meno comune
{% endhighlight %}

Due opzioni permettono di isolare solo un tipo di riga, invece di eliminare semplicemente i duplicati:

{% highlight shell %}
sort nomi.txt | uniq -d      # mostra SOLO le righe che compaiono più di una volta
sort nomi.txt | uniq -u       # mostra SOLO le righe che compaiono esattamente una volta
{% endhighlight %}

### cut: estrarre porzioni di ogni riga

Simile ad `awk` ma più semplice, `cut` estrae colonne o intervalli di caratteri da ogni riga:

{% highlight shell %}
cut -d: -f1 /etc/passwd     # estrae il primo campo (il nome utente), usando ":" come separatore
cut -c1-5 appunti.txt         # estrae i primi 5 caratteri di ogni riga
{% endhighlight %}

`-d` indica il carattere separatore (delimiter), `-f` il numero di campo da estrarre; `-c` estrae invece un intervallo di caratteri per posizione, indipendentemente da eventuali separatori. Si possono indicare anche più campi o intervalli insieme:

{% highlight shell %}
cut -d: -f1,3 /etc/passwd     # estrae il primo e il terzo campo (nome utente e UID)
cut -d, -f2-4 dati.csv          # estrae dal secondo al quarto campo di un CSV
cut -d: -f1 --complement /etc/passwd   # estrae tutti i campi TRANNE il primo
{% endhighlight %}

### tr: sostituire o eliminare caratteri

`tr` (*translate*) lavora a livello di singoli caratteri, non di righe intere, ed è pensato per essere usato in una pipe (non accetta un nome di file come argomento):

{% highlight shell %}
cat appunti.txt | tr 'a-z' 'A-Z'     # converte tutto il testo in maiuscolo
cat appunti.txt | tr -d ' '           # elimina tutti gli spazi
cat dati.csv | tr ',' ';'              # sostituisce ogni virgola con un punto e virgola
{% endhighlight %}

`tr` accetta anche delle classi di caratteri predefinite, utili quando l'insieme di caratteri da trattare è ampio:

{% highlight shell %}
cat appunti.txt | tr -d '[:digit:]'        # elimina tutte le cifre
cat appunti.txt | tr -d '[:punct:]'         # elimina tutta la punteggiatura
cat appunti.txt | tr -s ' '                  # riduce sequenze di più spazi consecutivi a uno solo (squeeze)
{% endhighlight %}

`-s` (*squeeze*) è particolarmente utile per "ripulire" un testo con spaziatura irregolare prima di elaborarlo con altri comandi come `cut` o `awk`.

### Un esempio che combina più comandi

Un caso tipico in cui questi strumenti si usano insieme: trovare le 3 parole più frequenti in un file di testo.

{% highlight shell %}
cat appunti.txt | tr ' ' '\n' | sort | uniq -c | sort -rn | head -n 3
{% endhighlight %}

Si legge così: `tr` trasforma ogni spazio in un ritorno a capo (una parola per riga), `sort` raggruppa le parole uguali, `uniq -c` le conta, `sort -rn` le ordina dalla più frequente, `head -n 3` ne mostra solo le prime tre. È un esempio della filosofia Unix: combinare piccoli strumenti specializzati con le pipe, invece di scrivere un programma dedicato per ogni compito.
