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
