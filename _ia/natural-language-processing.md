---
title: 'Natural Language Processing'
date: '2022-04-28T09:57:43+02:00'
author: Fabio Mattei
layout: page
---

# Che cosa significa NLP?

Il Natural language processing (NLP) si occupa di sviluppare applicazioni e servizi in grado di lavorare con il linguaggio umano. Esempi pratici sono i software di speech recognition: google voice search, siri, alexa. L’NLP si può anche utilizzare per fare delle sentiment analysis.

**Benefici del NLP**

Come tutti sappiamo ogni giorno vengono prodotti milioni di Gb di testo da blog, social network e pagine web. Ci sono molte compagnie che esplorano questi dati al fine di capire gli utenti e le loro passioni.  
Supponiamo che una persona che ama viaggiare faccia regolarmente ricerche per posti in cui andare in vacanza, le ricerche fatte da questo utente saranno utilizzate per proporre pubblicità che consiglino hotel e voli da prenotare. Ovviamente il marketing non è l’unica applicazione di queste tecnologie. Si possono scandagliare le comunicazioni umane per capire lo stato e l’evolversi di una malattia oppure per capire quali sono i problemi di una determinata area geografica.  
I motori di ricerca si avvalgono delle tecnologie di NLP per classificare le informazioni che gli spider trovano nella rete ma molte altre sono le tecnologie che fanno uso di queste tecniche.

**Implementazioni di NLP**

Alcuni esempi di implementazioni di Natural Language Processing (NLP):

- **Motori di ricerca** come Google, Yahoo, etc.
- **Social networks** come Facebook particolarmente applicato sugli aggiornamenti di stato. L’algoritmo esplora i post degli utenti e mostra loto pubblicità correlate e aggiornamenti di altri utenti che potrebbero essere interessati.
- **Speech engines** come Apple Siri, Amazon Alexa, Ok Google.
- **Filtri anti Spam** come Google spam filter. Per capire se un messaggio è indesiderato dobbiamo cercare di capire il suo contenuto.

# Cosa sono le caratteristiche di un testo (text features)

Il computer non ha la possibilità di comprendere il significato di un testo, non è intelligente come un uomo, ma ha la possibilità di fare indagini di tipo matematico/statistico. A tal fine scandaglia il testo in modo da generare informazione utilizzabile algoritmicamente o come input di una eventuale rete neurale.

**Numero di parole:**

La caratteristica più semplice consiste nel contare il numero di parole che compongono un testo.

Per tutti gli esempli seguenti si presupporrà che il testo sia contenuto in una variabile **campione.** campione = ’’’Il mio testo da analizzare’’’

{% highlight python %}
numero_di_parole = len(campione.split(" "))
{% endhighlight %}

**Numero di caratteri:**

{% highlight python %}
numero_di_caratteri = len(campione)
{% endhighlight %}

**Lunghezza media delle parole:**

{% highlight python %}
lunghezza_media = len(campione) / numero_di_parole
{% endhighlight %}

**Numero di parole maiuscole:**

{% highlight python %}
parole = campione.split(" ")
cont = 0
for parola in parole:
    if parola.isupper():
        cont = cont + 1
{% endhighlight %}

# Prepariamo il testo

Al fine di lavorare con il testo è bene renderlo tutto minuscolo. Questo facilita molto le operazioni che verranno in seguito

{% highlight python %}
punteggiatura = "’!()-\[\]{};:'”\\,<>;./?@#$%^&\*\_~"
for x in campione.lower():
    if x in punteggiatura:
        string = string.replace(x, "")
{% endhighlight %} 

**Rimuoviamo le parole comuni:**

Le parole comuni sono poco informative dal punto di vista statistico dunque è preferibile rimuoverle.

{% highlight python %}
parole_comuni = (‘a’, ‘e’, ‘è’, ‘di’, ‘in’, ‘che’, ‘un’, ‘in’)
campione_pulito = ""
for x in campione.lower():
    if x not in parole_comuni:  
        campione_pulito = campione_pulito + x
{% endhighlight %} 

**Contare la frequenza delle parole: (tokenization)**

{% highlight python %}
conteggio = dict()
parole = campione.split(" ")
for parola in parole:
    if parole in conteggio:
        conteggio[parola] += 1
    else:
        conteggio[parola] = 0
{% endhighlight %} 

**Parole derivate: (stemming)**

Al fine del conteggio statistico le parole derivate aggiungono poca informazione e rendono i calcoli più difficile dunque è meglio prendere dei provvedimenti.

Per parole derivate intendiamo le parole derivanti dal rendere una parola plurale (barca, barche) oppure dalla coniugazione di un verbo (leggo, leggi, legge, leggiamo).

Le parole “barca” e “barche” vengono considerate diverse dal computer ma noi sappiamo che sono collegate allo stesso significato. Dobbiamo dunque correre ai ripari per mitigare il problema per quanto possibile.

**N-grammi**

Gli N-grammi sono combinazioni di N parole usate insieme. Se N=1 si parla di unigramma, se N = 2 si parla di bigramma, se N = 3 si parla di trigramma ecc.  
Le normali tecniche di conteggio di parole consistono nel conteggio degli unigrammi, ma conteggiando bigrammi o trigrammi possiamo ottenere informazione più interessante.

Può essere interessante anche dividere il testo in frasi (frasi = campione.split(“.”)) e ripetere l’analisi delle caratteristiche di un testo analizzando frase dopo frase.

# Mettere a frutto i risultati di una analisi

Dopo aver estratto da un testo tutta la conoscenza che è possibile estrarre bisogna capire come mettere a frutto tale conoscenza. A tal fine si possono utilizzare diverse tecniche. Nessuna di queste ci assicura la correttezza del risultato ma possiamo accontentarci di cercare di migliorare la nostra accuratezza attraverso approssimazioni successive.

Una tecnica molto semplice consiste nel conteggiare il numero di parole e la loro cardinalità media all’interno di un certo numero di testi appartenenti ad una certa classe. In un secondo momento analizzeremo la presenta di queste parole in un nuovo testo campione al fine di determinare se il nuovo testo appartiene alla classe in esame oppure no.