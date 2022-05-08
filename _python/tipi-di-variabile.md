---
id: 207
title: 'Tipi di variabile'
date: '2020-02-07T22:03:41+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=207'
---

Una informazione, conservata in una variabile, ha sempre un tipo associato. **Il tipo della variabile determina l’insieme di valori che una variabile può assumere e le operazioni che possono manipolare tali valori**.

- interi (int) Es: 1, 5, 7, 1983, 20003
- numeri razionali (float) Es 1.2, 1.4, 6.7, 7.0
- numeri complessi (complex) Es. 2+5j, 2+8j, -4+2j, -5-3j
- valori booleani (bool) Es. True, False
- valori stringa (str) Es. “ciao”, ‘Buongiorno’

#### Operazioni su numeri interi, numeri a virgola mobile e numeri complessi

Per assegnare un numero ad una variabile basta utilizzare l’operatore = come visto nella pagina dedicata alle [variabili](https://www.esercizidiinformatica.it/variabili/).

{% highlight python %}
M = 3          # assegna ad M il numero intero 3 
M = 3.0        # assegna ad M il numero razionale (float) 3.0  
M = ( 3 + 1j ) # assegna ad M il numero complesso 3 + 1i
{% endhighlight %}

Le operazioni possibili su di una variabile di tipo **int**, **float** o **complex** sono:

- M+M → somma (interi, float, complessi)
- M\*M → prodotto (interi, float, complessi)
- M/M → divisione con risultato float, o complesso
- M//M → quoziente intero
- M%M → modulo (resto della divisione intera, solo tra interi)
- M\*\*M → potenza (interi, float, complessi)

######Esercizio 1: 
copia il seguente codice nell’editor. Una volta finito fallo eseguire premendo F5 oppure dando il comando RUN → RUN MODULE.

{% highlight python %}
a=2
b=3
area=a*b
perimetro=a*2+b*2
print(area,perimetro)
{% endhighlight %}

#### Operazioni sulle stringhe di testo

In informatica una stringa di testo è **una sequenza di caratteri con un ordine stabilito**. Facciamo qualche esempio:

{% highlight python %}
M = "Prova" 
N = "casa" 

{% endhighlight %}

Le operazioni possibili su di una variabile stringa sono:

- M+N → concatena la stringa M ed N (es. “Provacasa”)
- M\*3 → concatena 3 volte la stringa M (es. “ProvaProvaProva”)
- len(M) → restituisce la lunghezza di M (es. 5)
- M\[0\],···M\[len(M)-1\] → restituisce i singoli caratteri della stringa. es: (M\[0\]→P)

{% highlight python %}
nome = "Giacomo"              # assegnamento
cognome = "Leopardi"          # assegnamento
nome_cognome = nome + cognome # concatenazione "GiacomoLeopardi"
nome_ripatuto = nome * 3      # ripetizione "GiacomoGiacomoGiacomo"
lunghezza = len(nome)         # lunghezza 7
iniziale = nome[0]            # carattere G
{% endhighlight %}

######Esercizio 2: 
copia il seguente codice nell’editor e fallo eseguire

{% highlight python %}
a=2
b=3
area=a*b
perimetro=a*2+b*2
print(area,perimetro)
{% endhighlight %}

######Esercizio 3: 
copia il seguente codice nell’editor e fallo eseguire

{% highlight python %}
a = " c i a o "
b = " mondo "
stringaconcatenata = a + b
print( stringaconcatenata )
{% endhighlight %}

In questi esempi incontriamo per la prima volta l’istruzione [print](https://www.esercizidiinformatica.it/print/) che ci permette di stampare sul video il valore di una variabile.

######Esercizio 4: 
Stampare a video il perimetro di un quadrato avente lato l=4

######Esercizio 5: 
Stampare a video l’area di un quadrato avente lato l=5

######Esercizio 6: 
Stampare a video n volte, con n=10, la stringa s   
 (es. con s=”ciao” stamperà “ciaociaociaociaociaociaociaociaociaociao”)

######Esercizio 7: 
Stampare a video una stringa lunga 4 caratteri al contrario (es. se s=”lodi”, il programma stampa “idol”)

######Esercizio 8: 
Supponete di correre 10 km in 42 min e 42 sec. Stampate la vostra velocità media in km/minuto, km/h, miglia/minuto e miglia/h.

- Calcolate quanti secondi ci sono in 42 minuti e 42 secondi.
- A quante miglia corrispondono 10 km? (Suggerimento: ci sono 1,61 km in un miglio)
- La vostra velocità media è calcolata come distanza/tempo
