---
id: 105
title: 'La condizione'
date: '2020-02-04T12:17:23+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=105'
---

## Il costrutto if

Il costrutto **if**
ci consente di cambiare la sequenza logica di istruzioni da eseguire in un programma. L’istruzione successiva viene eseguita se la condizione risulta verificata.

#### Esercizio 1:
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
a = 33
b = 200
if b > a:
   print("b e' maggiore di a")
{% endhighlight %}

#### Esercizio 2:
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
a = 33
b = 200
if b > a:
print("b e' maggiore di a") # questo codice è errato dato che manca l'indentazione
{% endhighlight %}

In Python l’indentazione (gli spazi che nell’esempio 1 precedono l’istruzione print) indica l’appartenenza di un codice ad un blocco di codice. Dato che bisogna distinguere tra quali istruzioni eseguire in caso la condizione venga verificata oppure no, chi ha inventato il linguaggio ha deciso di utilizzare l’indentazione a tale scopo. Dunque nel primo esempio è chiaro per il computer che deve eseguire l’istruzione print() se b &gt; a, nel secondo esempio, mancando indentazione non è chiaro, quindi il computer ci dà errore.

    ####  Operatori di confronto
    
    Sono gli operatori che possiamo utilizzare all’interno di una condizione.
    
    - Uguale: a == b
    - Diverso: a != b
    - Minore: a < b
    - Minore o uguale: a <= b
    - Maggiore: a > b
    - Maggiore o uguale: a >= b

##  Il costrutto elif

Importante per dare una alternativa in caso la proposizione contenuta nel primo if non venga verificata.

#### Esercizio 3:
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
a = 33
b = 33
if b > a:
    print("b e' piu' grande di a")
elif a == b:
    print("a e b sono uguali")
{% endhighlight %}

## Il costrutto else

Nel caso nessuna delle proposizione contenute nelle condizioni precedenti sia stata verificata, si esegue il codice contenuto nel blocco else:

#### Esercizio 4:
copia il seguente codice nell’editor. Una volta finito fallo eseguire.

{% highlight python %}
a = 33
b = 33
if b > a:
    print("b e' piu' grande di a")
elif a == b:
    print("a e b sono uguali")
else:
    print("a e' piu' grande di b")
{% endhighlight %}

#### Esercizio 5:
scrivi un programma che lette due stringhe di testo ne scriva la prima in ordine alfabetico

#### Esercizio 6:
scrivi un programma che letti due numeri scriva il più grande tra i due

#### Esercizio 7:
scrivi un programma che letto un numero intero determini se è pari o dispari (utilizzare l’operatore resto: %)

#### Esercizio 8:
Dati 4 numeri in input determinare se la somma dei primi due è minore o uguale alla somma del terzo e del quarto

#### Esercizio 9:
Un’automobile percorre 20 km con un litro di benzina. Calcolare la spesa necessaria a percorrere 100 km. Se la spesa è maggiore di €100, applicare uno sconto del 5%

#### Esercizio 10:
Letti i lati di un triangolo determinare se è scaleno, isoscele o equilatero

#### Esercizio 11:
Letti due numeri naturali A e B, sottrarre il più piccolo dal più grande.

#### Esercizio 12:
Letti due numeri determinare se sono entrambi compresi tra 100 e 200

#### Esercizio 13:
Letto un numero intero, trovare il suo valore assoluto.

#### Esercizio 14:
Letti due numeri interi A e B verificare se A è il quadrato di B

#### Esercizio 15:
Un’azienda elettrica ha stabilito le seguenti tariffe:

| KW/H         |                                                                                   |
|--------------|-----------------------------------------------------------------------------------|
| 0 – 500      | 20                                                                                |
| 501 – 1000   | 20 + 0,08 per ogni KW/H oltre i 500                                               |
| 1001 – oltre | 20 + 0,08 per ogni KW/H compreso tra 500 e 1000 + 0,05 per ogni KW/H oltre i 1000 |

Scrivere un programma che letto il consumo mensile calcoli e stampi l’importo della bolletta.

#### Esercizio 16:
Scrivi un programma python che legga il valore di una spesa e calcoli lo sconto secondo la seguente tabella:

| Spesa                |  Sconto        |
|----------------------|----------------|
| Al di sotto di 100 € | nessuno sconto |
| Tra 100 e 300        | sconto del 10% |
| Tra i 300 e i 500    | sconto del 15% |
| Tra i 500 e i 800    | sconto del 20% |

#### Esercizio 17:
Simona deve comperare le paste per il suo bar. 
Le tariffe applicate dal pasticciere sono le seguenti: Se le paste sono fino a 20 il prezzo è di 1 euro 
per ciascuna pasta Se le paste sono più di 20 ma meno di 40 il prezzo è di 0.9 euro per pasta.
Se le paste sono più di 40 ma meno di 60 il prezzo è di 0.8 euro per pasta Se le paste sono più di 100 
il prezzo è di 0.6 euro per pasta Scrivi il programma che aiuta il panettiere a calcolare l'ammontare che 
Simona dovrà pagare.

#### Esercizio 18:
Tenendo conto degli scaglioni fiscali definiti correntemente:

| Reddito         | Aliquota |
|-----------------|----------|
| 0 - 15000 €     | 23%      |
| 15001 - 28000 € | 25%      |
| 28001 - 50000 € | 35%      |
| 50001 in su   € | 43%      |

Scrivere un programma che letto il reddito di 5 cittadini italiani calcoli l'ammontare delle tasse che ciascun cittadino deve pagare ed il totale pagato da tutti i cittadini.



