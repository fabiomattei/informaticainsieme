---
title: 'Shell scripting in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Uno **shell script** è un file di testo contenente una sequenza di comandi bash, eseguibili tutti insieme invece di digitarli uno per uno. È lo strumento con cui su Linux si automatizzano le operazioni ripetitive.

### Un primo script

{% highlight shell %}
#!/bin/bash
echo "Ciao, mondo!"
{% endhighlight %}

La prima riga, chiamata **shebang**, indica al sistema quale interprete usare per eseguire il file (in questo caso bash). Per eseguire lo script bisogna prima renderlo eseguibile, poi lanciarlo:

{% highlight shell %}
chmod +x saluto.sh
./saluto.sh
{% endhighlight %}

### Variabili

{% highlight shell %}
#!/bin/bash
nome="Marco"
echo "Ciao, $nome!"
{% endhighlight %}

A differenza di molti linguaggi di programmazione, tra il nome della variabile e il segno `=` non vanno messi spazi. Per usare il valore di una variabile si antepone il simbolo `$`.

### Argomenti passati allo script

{% highlight shell %}
#!/bin/bash
echo "Il primo argomento è: $1"
echo "Il numero di argomenti è: $#"
{% endhighlight %}

Eseguito con `./script.sh ciao`, `$1` conterrà `ciao`. `$0` contiene invece il nome dello script stesso.

### Condizioni: if

{% highlight shell %}
#!/bin/bash
eta=20

if [ $eta -ge 18 ]; then
  echo "Maggiorenne"
else
  echo "Minorenne"
fi
{% endhighlight %}

Gli operatori di confronto tra numeri più comuni sono `-eq` (uguale), `-ne` (diverso), `-lt` (minore), `-le` (minore o uguale), `-gt` (maggiore), `-ge` (maggiore o uguale). Per confrontare stringhe di testo si usano invece `=` e `!=`.

### Cicli: for e while

{% highlight shell %}
#!/bin/bash
for file in *.txt; do
  echo "Trovato: $file"
done
{% endhighlight %}

Il ciclo `for` qui scorre tutti i file con estensione `.txt` nella cartella corrente. Un ciclo `while` ripete invece finché una condizione resta vera:

{% highlight shell %}
#!/bin/bash
contatore=1

while [ $contatore -le 5 ]; do
  echo "Ciclo numero $contatore"
  contatore=$((contatore + 1))
done
{% endhighlight %}

### Catturare l'output di un comando

Il risultato di un comando può essere salvato in una variabile racchiudendolo tra `$( )`:

{% highlight shell %}
#!/bin/bash
data_corrente=$(date +%Y-%m-%d)
echo "Oggi è il $data_corrente"
{% endhighlight %}

### Uno script più completo

{% highlight shell %}
#!/bin/bash
# Esegue un backup della cartella indicata come argomento

cartella=$1
destinazione="backup_$(date +%Y%m%d).tar.gz"

if [ -d "$cartella" ]; then
  tar -czf "$destinazione" "$cartella"
  echo "Backup completato: $destinazione"
else
  echo "Errore: la cartella $cartella non esiste"
fi
{% endhighlight %}

### case: scegliere tra più alternative

Quando le condizioni da confrontare sono molte, `case` è più leggibile di una lunga catena di `if`/`elif`:

{% highlight shell %}
#!/bin/bash
read -p "Scegli un'opzione (avvia/ferma/stato): " opzione

case $opzione in
  avvia)
    echo "Avvio il servizio..."
    ;;
  ferma)
    echo "Fermo il servizio..."
    ;;
  stato)
    echo "Il servizio è attivo."
    ;;
  *)
    echo "Opzione non riconosciuta"
    ;;
esac
{% endhighlight %}

Ogni blocco termina con `;;`; l'asterisco `*` cattura tutti i casi non previsti esplicitamente, in modo analogo al `default` di altri linguaggi.

### Array

Bash supporta anche array semplici, utili per raccogliere più valori in una sola variabile:

{% highlight shell %}
#!/bin/bash
distribuzioni=("Ubuntu" "Debian" "Fedora" "Arch")

echo "Prima distribuzione: ${distribuzioni[0]}"
echo "Totale: ${#distribuzioni[@]}"

for d in "${distribuzioni[@]}"; do
  echo "- $d"
done
{% endhighlight %}

Gli indici partono da 0; `${#array[@]}` restituisce il numero di elementi, `${array[@]}` l'elenco completo.

### Funzioni

Per riutilizzare porzioni di script senza ripeterle, si possono definire delle funzioni:

{% highlight shell %}
#!/bin/bash

saluta() {
  local nome=$1
  echo "Ciao, $nome!"
}

saluta "Marco"
saluta "Giulia"
{% endhighlight %}

La parola chiave `local` rende la variabile visibile solo all'interno della funzione, evitando che il suo valore interferisca con variabili omonime usate altrove nello script.

### Codici di uscita e gestione degli errori

Ogni comando, al termine dell'esecuzione, restituisce un **codice di uscita**: `0` indica successo, qualsiasi altro valore (da 1 a 255) indica un errore. Questo codice è accessibile tramite la variabile speciale `$?`:

{% highlight shell %}
#!/bin/bash
mkdir progetti
echo "Codice di uscita di mkdir: $?"
{% endhighlight %}

È buona pratica interrompere uno script non appena un comando fallisce, invece di proseguire in uno stato inconsistente. L'istruzione `set -e`, posta all'inizio dello script, fa esattamente questo:

{% highlight shell %}
#!/bin/bash
set -e   # interrompe lo script al primo comando che fallisce

cd /cartella/che/potrebbe/non/esistere
rm -rf *
{% endhighlight %}

Senza `set -e`, se `cd` fallisse (ad esempio perché la cartella non esiste), lo script proseguirebbe comunque con `rm -rf *` eseguito nella cartella sbagliata: un errore potenzialmente distruttivo. Con `set -e`, lo script si ferma subito dopo il fallimento di `cd`.

### Debug di uno script

Quando uno script non si comporta come previsto, `set -x` mostra ogni comando eseguito (con le variabili già espanse) prima di eseguirlo, utile per capire esattamente cosa sta succedendo passo per passo:

{% highlight shell %}
#!/bin/bash
set -x
nome="Marco"
echo "Ciao, $nome!"
{% endhighlight %}

In alternativa, senza modificare lo script, lo stesso effetto si ottiene lanciandolo con:

{% highlight shell %}
bash -x script.sh
{% endhighlight %}
