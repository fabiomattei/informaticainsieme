---
title: Il contatore
date: '2024-06-13T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

Livello: **iniziale**

Nozioni da conoscere per poter affrontare la lezione: **variabile, condizione, input**

## Il computer è stato creato per contare

Contare sembra a noi una azione molto semplice una cosa che abbiamo imparato a fare durante le prime fasi dell'infanzia e che diamo ormai per assodato. I bambini contano quando giocano al gioco della campana e contano quando giocano a nascondino e utilizzano la loro piccola mano aiutarsi a tenere il conto.
Finché c'è la necessità di contare poche cose questa azione sembra semplice ma cosa succede quando devo contare **milioni di cose** oppure **milioni di cittadini**? Questa azione ci costerebbe una gran fatica!

![Herman Hollerith](/images/algoritmi/contatore/hollerith.jpg){:class="aside-image"}

In effetti uno dei problemi che ha fatto sentire all'uomo esigenza di creare dei meccanisimi automatici di calcolo è stato quello del conteggio. Più precisamente ci riferiamo al conteggio dei cittadini per il censimento degli Stati Uniti del 1890. A tal fine, intorno al 1880, Herman Hollerith sviluppa l'idea di elaborare dati usando schede perforate. Il sistema fa passare schede perforate sopra opportuni contatti elettrici e riesce a compilare elaborazioni statistiche. Possiamo dunque affermare che uno dei principali problemi che si cerca di risolvere attraverso l'uso dei computer è quello del conteggio.

## Il contatore ed il conteggio

Come abbiamo detto in precedenza, i bambini quando contano si aiutano con le mani e aprono le dita una dopo l'altra per aiutarsi a ricordare il valore raggiunto nel conteggio. Allo stesso modo quando andiamo ad implementare un conteggio, ci avvaliamo dell'uso di una **variabile** che **porta il conto**, che memorizza cioè al suo interno la quantità contata fino a quel momento. Questa variabile è dunque come le altre variabili un **contenitore**, appartiene al **tipo intero** e ci permette di conservare memoria dello stato del conteggio di momento in momento.

## Contiamo sulla base di informazioni inserite dall'utente

Leggiamo tre informazioni dall'utente e contiamo quante volte questi ha inserito la stringa di testo "ciao".

{% highlight python %}
contatore = 0
  
parola1 = input()
if parola1 == "ciao":
    contatore = contatore + 1

parola2 = input()
if parola2 == "ciao":
    contatore = contatore + 1
	
parola3 = input()
if parola3 == "ciao":
    contatore = contatore + 1
{% endhighlight %}

Quando andiamo a strutturare un conteggio è bene **inzializzare** un contatore. Inizializzare significa **impostare il contatore ad un valore noto iniziale** in modo da essere sicuri su quale sia il valore dal quale inizia il conteggio. In questo caso si è deciso di impostare il contatore a zero.

In seguito il programma legge una stringa di testo dall'utente, la confronta con la stringa di testo "ciao" e, nel caso queste siano uguali, incrementa il contatore.

Notiamo una cosa molto strana, una cosa che distingue l'informatica dalla matematica: contatore = contatore + 1.
Cosa significa?
**Assegno alla variabile contatore il contenuto della variabile contatore incrementato di uno**
Questo vuol dire che il computer va a controllare il contenuti della variabile contatore, lo incrementa di una unità e assegna questo nuovo valore alla variabile contatore. In questo modo il contenuto della variabile contatore viene incrementato di uno. In pratica abbiamo incrementato il contatore.

## Cosa succede se vogliamo aumentare le informazioni che l'utente inserisce?

Leggiamo sette informazioni dall'utente e contiamo quante volte questi ha inserito la stringa di testo "ciao".

{% highlight python %}
contatore = 0
  
parola1 = input()
if parola1 == "ciao":
    contatore = contatore + 1

parola2 = input()
if parola2 == "ciao":
    contatore = contatore + 1
	
parola3 = input()
if parola3 == "ciao":
    contatore = contatore + 1
	
parola4 = input()
if parola4 == "ciao":
    contatore = contatore + 1
	
parola5 = input()
if parola5 == "ciao":
    contatore = contatore + 1
	
parola6 = input()
if parola6 == "ciao":
    contatore = contatore + 1
	
parola7 = input()
if parola7 == "ciao":
    contatore = contatore + 1
{% endhighlight %}

Questa è un software che risolve il problema proposto in modo corretto. Il problema che notiamo è che diventa estremamente tedioso se abbiamo il bisogno di estenderlo. Cosa succede se dobbiamo contare quante volte su 100 inserimenti di informazione l'utente ha inserito la parola ciao?

## Contatori e cicli

Esiste un modo più intelligente per risolvere questo problema: basarci su di un ciclo.

{% highlight python %}
contatore = 0
  
ripetizioni = 1
while ripetizioni <= 100:
    parola = input()
	if parola == "ciao":
	    contatore = contatore + 1
	ripetizioni = ripetizioni + 1
{% endhighlight %}

In questo caso notiamo la presenza di due variabili contatore:

* contatore (int): tiene memoria di quante occorrenze della parola "ciao" sono state contate;
* ripetizioni (int): tiene memoria di quante volte è stata ripetuta l'operazione di input e il successivo confronto.

Entrambe sono variabili contatore ma hanno un diverso compito.

Il ciclo, basandosi sulla variabile **ripetizioni** esegue 100 volte l'operazione di lettura (input) e il confronto con la stringa di testo "ciao" da cui dipende l'incremento della variabile **contatore**.


