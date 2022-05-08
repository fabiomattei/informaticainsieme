---
title: Input
date: '2020-01-22T09:29:54+01:00'
author: fabio
layout: page
---

La funzione **input** viene usata per consentire all’utente di immettere dati da tastiera, che verranno poi utilizzati dal programma. **Tali dati vanno memorizzati in una variabile** altrimenti vengono *dimenticati* immediatamente dal computer.

**input** accetta un singolo argomento opzionale: una *stringa di testo* che viene mostrata a video prima di leggere il valore digitato. Questa ha la funzione di dare una indicazione all’utente sull’informazione che si vuole ottenere da lui. Una volta che l’utente abbia digitato un valore e premuto il tasto *Invio*, la funzione input restituisce il valore come *stringa*, come mostra il seguente esempio:

{% highlight python %}
nome = input('Inserisci il tuo nome: ')
# Il computer scrive sul video "Inserisci il tuo nome: "
# quindi rimane in attesa che l'utente scriva qualcosa.
# Supponiamo che l'utente scriva la stringa "Ezio"
print(nome) # stampa 'Ezio'
{% endhighlight %}

#### Esercizio 1: 
copia il seguente codice nell’editor. Una volta finito fallo eseguire dal computer premendo F5 oppure dando il comando RUN

{% highlight python %}
nome = input("scrivi il tuo nome: ")
print("Ciao ", nome)
{% endhighlight %}

#### Esercizio 2: 
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
nome = input("scrivi il tuo nome: ")
cognome = input("scrivi il tuo cognome: ")
print("Nome: ", nome, " cognome: ", cognome)
{% endhighlight %}

Abbiamo detto che per conservare le informazioni che l’utente digita il computer ha bisogno di [varabili](https://www.esercizidiinformatica.it/variabili/). Nell’esercizio precedente abbiamo richiesto all’utente due informazioni, la prima l’abbiamo conservata all’interno della variabile *nome*, la seconda all’interno della variabile *cognome*.

#### Esercizio 3: 
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
nome = input("scrivi il tuo nome: ")
lunghezza = len(nome)
print("Il tuo nome e’ lungo ", lunghezza, " caratteri")
{% endhighlight %}

La funzione input, abbiamo detto restituisce il valore come una stringa di testo.

Nel caso in cui vogliamo chiedere all’utente un valore numerico abbiamo bisogno di cambiare il valore di tipo attraverso la funzione **int()**.

#### Esercizio 4: 
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
valorestringa = input("scrivi la lughezza del lato: ")
lato = int(valorestringa)
area = lato * lato
print("L’area del quadrato di lato ", lato, " vale ", area)
{% endhighlight %}

#### Esercizio 5: 
scrivi un programma che letta una variabile intera calcoli l’area del cerchio che ha per raggio il valore appena richiesto.

#### Esercizio 6: 
scrivi un programma che letta una parola calcoli l’area del cerchio che ha per raggio il la lunghezza della parola appena richiesta

#### Esercizio 7: 
scrivi un programma che letta una variabile intera indicante un tempo in ore, calcoli di quanti minuti è composto quel tempo

#### Esercizio 8: 
scrivi un programma che letta una variabile intera indicante un tempo in ore, calcoli di quanti secondi è composto quel tempo

Qualche volta bisogna richiedere in input dei valori in virgola mobile in quel caso si usa la funzione **float()**

#### Esercizio 9: 
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
valorestringa = input("scrivi la lughezza del lato: ")
lato = float(valorestringa)
area = lato * lato
print("L’area del quadrato di lato ", lato, " vale ", area)
{% endhighlight %}

####  Operatori matematici:

–, +, -, / (divisione), \* (moltiplicazione), \*\* (esponente), // (divisione intera), % (resto della divisione intera).

#### Esercizio 10: 
scrivi un programma che letti due valori interi calcoli le operazioni che si ottengono utilizzando tutti gli operatori matematici elencati e scriva il risultato

#### Esercizio 11: 
scrivi un programma che letti due valori a virgola mobile calcoli le operazioni che si ottengono utilizzando tutti gli operatori matematici elencati e scriva il risultato

#### Esercizio 12: 
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
capitaleiniziale = int(input("Capitale iniziale: "))
tasso = int(input("Tasso: "))
anni = int(input("Anni: "))
capitalefinale = capitaleiniziale * (1+ tasso) ** anni
print("Dopo  ", anni, " anni il capitale e’ ", capitalefinale)
print("Dopo  ", anni, " anni il capitale e’ ", round(capitalefinale, 2))
{% endhighlight %}

La funzione **round(valore, cifra)** arrotonda un valore ad una certa cifra decimale.

#### Esercizio 13: 
scrivi un programma che letta la distanza e il tempo calcoli la velocità di un corpo arrotondata alla terza cifra decimale. (Velocità = spazio / tempo)

#### Esercizio 14: 
scrivi un programma che letto un numero rappresentante un valore imponibile calcoli l’iva e scriva il totale composto da imponibile sommato all’iva.

#### Esercizio 15: 
scrivi un programma che letto un valore in euro scriva il valore di vecchie lire corrispondente. (1 euro = 1927,36 lire)

#### Esercizio 16: 
scrivi un programma che letta una stringa di testo ed un numero intero, scriva la stringa di testo tante volte quanto è grande il numero.
