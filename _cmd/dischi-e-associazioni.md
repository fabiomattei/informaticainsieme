---
title: 'Dischi, formattazione e associazioni di file'
date: '2026-07-26T10:40:00+02:00'
author: Fabio Mattei
layout: page
---

Questa pagina raccoglie comandi per gestire i dischi collegati al computer e le associazioni tra file e programmi. Alcuni di questi comandi, se usati con distrazione, possono **cancellare dati in modo permanente**: leggi con attenzione gli avvisi prima di provarli, e usali solo su dischi di cui sei sicuro (per esempio una chiavetta USB personale, mai un disco che non è tuo).

## vol - mostra etichetta e numero di serie di un disco

{% highlight shell %}
vol C:
{% endhighlight %}

Mostra il nome (etichetta) assegnato al disco `C:` e il suo numero di serie, un codice univoco assegnato da Windows.

## label - cambia l'etichetta di un disco

{% highlight shell %}
label D: ChiavettaUSB
{% endhighlight %}

Assegna il nome "ChiavettaUSB" al disco `D:`. Questo nome comparirà poi in Esplora file accanto alla lettera del disco.

## diskpart - gestione avanzata di dischi e partizioni

**Attenzione: `diskpart` è uno strumento molto potente, capace di cancellare intere partizioni o dischi in pochi secondi e senza possibilità di annullare. Usalo solo se sai esattamente cosa stai facendo, e controlla sempre due volte il numero del disco su cui stai per agire.**

`diskpart` funziona in modo diverso dagli altri comandi visti finora: digitandolo si entra in un vero e proprio programma con un proprio prompt (`DISKPART>`), dentro cui si digitano altri comandi specifici, fino a uscire con `exit`.

{% highlight shell %}
diskpart
{% endhighlight %}

Una volta dentro, per vedere l'elenco dei dischi collegati (in sola lettura, nessun rischio):

{% highlight shell %}
DISKPART> list disk
{% endhighlight %}

Per vedere le partizioni del disco attualmente selezionato:

{% highlight shell %}
DISKPART> list volume
{% endhighlight %}

Questi due comandi (`list disk` e `list volume`) si possono usare in tutta tranquillità per esplorare la situazione, perché si limitano a mostrare informazioni. I comandi che invece modificano o cancellano qualcosa (come `select disk`, `clean` o `create partition`) vanno usati solo dopo aver capito bene con quale disco si sta lavorando, perché **`clean` cancella tutte le partizioni del disco selezionato senza chiedere conferma**.

Per uscire da `diskpart` e tornare al Prompt normale:

{% highlight shell %}
DISKPART> exit
{% endhighlight %}

## format - formatta un disco

**Attenzione: `format` cancella tutti i dati presenti nel disco o nella chiavetta indicati. Prima di eseguirlo, controlla con `dir` o da Esplora file che la lettera scelta sia davvero quella giusta.**

{% highlight shell %}
format D:
{% endhighlight %}

Prepara il disco `D:` con un file system pulito e vuoto, cancellando tutto ciò che conteneva prima. Si usa tipicamente per preparare una chiavetta USB da riutilizzare da zero.

Alcune opzioni utili:

{% highlight shell %}
format D: /fs:NTFS      formatta usando il file system NTFS (quello usato normalmente da Windows)
format D: /q            esegue una formattazione "veloce" (quick), più rapida ma meno approfondita
{% endhighlight %}

## assoc - associazioni tra estensione e tipo di file

Ogni estensione di file (`.txt`, `.jpg`, `.bat`...) è collegata internamente a un "tipo di file", che Windows usa per decidere con quale programma aprirlo. Per vedere questa associazione:

{% highlight shell %}
assoc .txt
{% endhighlight %}

Per vedere l'elenco completo di tutte le associazioni:

{% highlight shell %}
assoc
{% endhighlight %}

## ftype - programma associato a un tipo di file

Mentre `assoc` collega un'estensione a un "tipo di file", `ftype` dice quale programma viene lanciato per quel tipo:

{% highlight shell %}
ftype txtfile
{% endhighlight %}

Per cambiare il programma associato (esempio: aprire i file di testo sempre con il Blocco note):

{% highlight shell %}
ftype txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
{% endhighlight %}
