---
title: Le stringhe di testo
date: '2020-02-07T22:32:45+01:00'
author: Fabio Mattei
layout: page
---

# Un susseguirsi di caratteri ascii

Una stringhe di testo altro non è che un susseguirsi di caratteri ascii. Il computer manipola soltanto numeri binari ed utilizza la tabella ascii per associare un numero a ciascun simbolo al fine di essere in grado di manipolarlo e lavorarci.

![Prima metà della tabella ASCII](/images/python/stringhe/asciisemi.gif)

Nello specifico vediamo che la porzione dedicata ai numeri e alle lettere è espressa nella tabella di fianco.

Questo significa che se vogliamo codificare la parola "CIAO" il computer memorizzerà una sequenza di codici ascii in questo modo:

|C  |I  |A   |O   |
| 01000011  | 01001001  | 01000001   | 01001111   |

Per questo motivo si dice che le stringhe di testo sono tipi di dato composti, in effetti sono composte da dati più piccoli: i caratteri.

# Inizializzare una stringa

{% highlight python %}
saluto = "ciao"
{% endhighlight %}

Assegnare una stringa di testo corrisponde al memorizzare i caratteri che la compongono all'interno di una area di memoria e ad etichettare quell'area di memoria attraverso un nome. L'istruzione precedente si legge: **assegno alla variabile saluto la stringha di testo ciao** e corrisponde proprio all'atto appena descritto. L'assegnamento si esprime attraverso l'operatore =.

# Lughezza di una stringa 

Dato che abbiamo visto che una stringha è composta da caratteri può capitare di voler contare i caratteri che la compongono. Facciamo questo attraverso la funzione len:

{% highlight python %}
len("ciao")
{% endhighlight %}

La funzione len restituisce **un numero intero** pari al numero di caratteri che sono contenuti nella stringa.

Potrei assegnare un numero intero ad una variabile

{% highlight python %}
numero_caratteri = len("ciao")
print(numero_caratteri)
{% endhighlight %}

In questo esempio la funzione len **conta** quanti caratteri sono contenuti nella stringa di testo (4) e assegna questo numero appena calcolato alla variabile **numero_caratteri**. 
In seguito la funzione print scrive sulla console il contenuti della variabile numero_caratteri.

# Accediamo ad un singolo carattere contenuto in una stringa

Qualche volta ci capita di avere necessità di controllare, o scrivere un singolo carattere all'interno della stringa. In quel caso utilizziamo l'operatore [].

{% highlight python %}
saluto = "ciao"
singola_lettera = saluto[1]
print(singola_lettera)
{% endhighlight %}

Python numera ciascun carattere contenuto in una stringa con un indice. Il conteggio inizia da 0 e finisce a n-1 dove n è il numero di caratteri che compongono la stringa.

|lettera|C  |I  |A   |O   |
|codice ascii| 01000011  | 01001001  | 01000001   | 01001111   |
|indice|0  |1  |2   |3   |

Il codice appena scritto dunque scrive la lettera **i** cioè il simbolo corrispondente all'indice 1 all'interno della stringa **saluto**.

# Confrontiamo le stringhe

Gli operatori di confronto per le stringhe di testo sono i seguenti:

* == (è uguale)
* \> (viene dopo di)
* \>= (viene dopo di o è uguale a)
* < (viene prima di)
* <= (viene prima di o è uguale a)
* != (è diverso)

{% highlight python %}
"ciao" == "ciao"       # True
"ciao" > "ciao"        # False
"mattino" > "sera"     # False
"mattino" > "mattina"  # False
{% endhighlight %}

Il confronto tra stringhe di testo avviene considerando un carattere per volta di ciascuna stringa.

E' possibile anche confrontare stringhe che sono all'interno di variabili

{% highlight python %}
saluto = "ciao"
momento = "mattino" 
saluto > momento      # False 
{% endhighlight %}

Ovviamente i confronti fra stringhe possono essere utilizzati nelle espressioni booleane che sono all'interno delle condizioni:

{% highlight python %}
saluto = "ciao"
momento = "mattino" 
if saluto > momento:
    print(saluto)
else: 
    print(momento)
{% endhighlight %}

#### Tabella di tracciamento

|saluto| momento  | output |
|"ciao"| "mattino"   | mattino   |

Devi comunque fare attenzione al fatto che Python non gestisce le parole maiuscole e minuscole come facciamo noi in modo intuitivo: in un confronto le lettere maiuscole vengono sempre prima delle minuscole.

# Visitiamo le stringhe

Attraverso l'uso degli indici possiamo visitare una stringha di testo, cioé possiamo considerare un carattere per volta all'interno di un ciclo per farci delle operazioni.

{% highlight python %}
saluto = "ciao"
indice = 0 
while indice < len(saluto): 
  lettera = saluto[indice]
  if lettera == "a" or lettera == "e" or lettera == "i" or lettera == "o" or lettera == "u":
     print("vocale")
  else:
     print("consonante")
  indice = indice + 1 
{% endhighlight %}

#### Tabella di tracciamento

|saluto| indice  | lettera  | ouput |
|"ciao"| 0  | "c"  | consonante   |
| |1  | "i"  | vocale   |
| |2  | "a"  | vocale   |
| |3  | "o"  | vocale   |

Possiamo scrivere la stessa cosa più semplicemente con un ciclo for:

{% highlight python %}
saluto = "ciao"
for lettera in saluto: 
  if lettera == "a" or lettera == "e" or lettera == "i" or lettera == "o" or lettera == "u":
     print("vocale")
  else:
     print("consonante")
{% endhighlight %}


## Esercizi

#### Esercizio 1:
Scrivi un programma Python che utilizzando la funzione print e il carattere asterisco legga 3 numeri e generi il relativo istogramma
Es: input 3, 5, 6
***
*****
******

{% highlight python %}
#Ricorda che:
nome = input("Scrivi il tuo nome:") # ritorna sempre una stringa di testo
len(nome)                           # ritorna la lunghezza della stringa
print("#"*3)                        # scrive 3 volte il carattere #
{% endhighlight %}

#### Esercizio 2:
Scrivi un programma Python che lette due stringhe le scriva in ordine di lunghezza (prima la più corta)

#### Esercizio 4:
Scrivi un programma Python che letta una stringa ne calcoli la lunghezza e la riscriva tante volta quanto è la sua lunghezza

#### Esercizio 5:
Scrivi un programma Python che lette due stringhe le scriva in ordine alfabetico [con le stringhe si possono utilizzare gli operatori < e >]

#### Esercizio 6:
Scrivi un programma Python che lette tre stringhe le scriva in ordine alfabetico

#### Esercizio 7:
Scrivi un programma Python che letto un numero intero n e una stringa s scriva la stringa s solo se la sua lunghezza è maggiore di n

#### Esercizio 8:
Tabella di tracciamento
{% highlight python %}
S="mandolino"
C=1
K=""
while C <= len(S):
      If C > 10:
          K = "X"
      else:
          K = "P"
     C=C+1
print(K)
{% endhighlight %}

#### Esercizio 9:
Scrivi un programma python che visiti una stringa di testo e scriva sul display “vocale” ogni volta che incontra una vocale e “consonante” ogni volta che incontra una consonante.


#### Esercizio 10:
Scrivi un programma python che legga due stringhe di testo e componga una nuova stringa di testo alternando i caratteri delle stringhe inziali.
Esempio
Str1: casa
Str2: rosa
Output: craossaa

#### Esercizio 11:
Scrivi un programma python che letta una stringa di testo conti quante vocali ci sono al suo interno.

#### Esercizio 12:
Scrivi un programma python che letta una stringa di testo crei una nuova stringa che sostituisca tutte le S della strina letta (maiuscole e minuscole) con il carattere $ e tutte le E (maiuscole e minuscole) con il carattere €.







#### Esercizio 13:
Scrivi un programma python che letto un numero calcoli la somma delle cifre che lo compongono. 
Es input = 124 output = 7

#### Esercizio 14:
Scrivi un programma python che crei una stringa di 10 vocali estratte casualmente e conti il numero di occorrenze di A al suo interno


#### Esercizio 15:
Generatore di password: Crea un programma che generi una password a lunghezza variabile casuale compresa tra 8 e 25

#### Esercizio 16:
Generatore di 10 password: Crea un programma che generi 10 password a lunghezza variabile casuale compresa tra 8 e 25

#### Esercizio 17:
Scrivi un programma che letta una stringa di testo messaggio ed un numero intero k (compreso tra 1 e 25) applichi alla stringa di testo messaggio l'algoritmo del cifrario di Cesare con chiave k.


#### Esercizio 18:
Copia il seguente codice in un file python e gioca con il seguente videogioco:

{% highlight python %}
import random

score = 0
attempts = 0
test_words = [
    "ciao mondo",
    "delfino",
    "pizzelle",
    "ciliegie",
    "motorino"
    ]
    
print ("""
        Benvenuto nel gioco delle stringhe fatte a fette (slicing)!
        
        Questo gioco ti mostra una stringha e una piccola espressione di slicing
        tu vinci insdovinando il risultato che l'espressione dovrebbe 
        Restituire.
        
        Scrivi "x" nel prompt per usicre. (Non ci sono stringhe con la "x").
    """)
    
while(1):
    attempts += 1
    word = test_words[ random.randrange(0,len(test_words)) ]
    start = random.randrange(0,len(word))
    end = random.randrange(0,len(word))
    
    if random.random() > 0.5:
        start = start * -1
    if random.random() > 0.5:
        end = end * -1
    
    if start > 0 and end > 0 and start > end:
        temp = start
        start = end
        end = temp
        
    if random.random() > 0.8:
        start = None
    if random.random() < 0.2:
        end = None
    
    ans = word[start:end]
    print (attempts, ": stringa = \"" + word + "\"")
    print ("    Calcola: ")
    if start == None:
        print ("    stringa[:" + str(end) + "]\n")
    elif end == None:
        print ("    stringa[" + str(start) + ":]\n")
    else:
        print ("    stringa[" + str(start) + ":" + str(end) + "]\n")
    
    inpt = input("=> ")
    if inpt == "x":
        print ("    Fine!!!!")
        print ("    ", score, " / ", attempts)
        print ("\n")
        break
        
    if inpt == ans:
        score += 1
        print ("    Successo! ... corretto: ", ans)
        print ("    ", score, " / ", attempts)
        print ("\n")
    else:
        print ("    Errore! ... la risposta corretta era: ", ans)
        print ("    ", score, " / ", attempts)
        print ("\n")
{% endhighlight %}
