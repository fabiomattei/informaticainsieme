---
title: 'I dizionari'
date: '2020-02-09T06:44:11+01:00'
author: Fabio Mattei
layout: page
---

Se le variabili sono cassetti e liste e tuple sono cassettiere, i dizionario sono cassettiere con etichette.

![Dizionari](/images/python/dizionari/cassettieraetichette.jpg){:class="aside-image"}

Un dizionario è una struttura dati che può accogliere molti dati. Ciascun dato è associato ad una etichetta, che d’ora in avanti chiameremo **chiave**, che ci permette di identificarlo in maniera univoca all’interno della struttura.

I dizionari sono un tipo **mutabile** e **non ordinato** che contiene elementi formati da una **chiave** e un **valore**. Si utilizza questa struttura dati quando non è comodo lavorare con un indice che sia un numero intero, ma si vuole utilizzare un indice di tipo diverso ad esempio testuale.

#### Definire i dizionari 

I dizionari vengono definiti elencando tra **parentesi graffe** una serie di elementi separati da **virgole**, dove ciascun elemento è formato da una chiave e un valore separati dal simbolo **due punti**. È possibile creare un dizionario vuoto usando le parentesi graffe senza alcun elemento al suo interno.

{% highlight python %}
d1 = {}                       # dizionario vuoto
d2 = {'a': 1}                 # contenente un elemento 
d3 = {'a': 1, 'b': 2, 'c': 3} # contenente 3 elementi

Ilmiodiz = {
  "lati": 6,
  "parola": "Saluti",
  "3x3": 9,
  "area": 4.3,
  13: "Panino",
}
{% endhighlight %}

In questo esempio possiamo vedere che d3 è un dizionario che contiene 3 elementi formati da una chiave e un valore. ’a’, ’b’ e ’c’ sono le chiavi, mentre 1, 2 e 3 sono i valori. Le chiavi di un dizionario sono solitamente stringhe, ma è possibile usare anche altri tipi. I valori possono essere di qualsiasi tipo.

{% highlight python %}
# tipo int per la chiave, tipo list per il valore
d = {20: ['Jack', 'Jane'], 28: ['John', 'Mary']} 

# non posso utilizzare il tipo list come chiave
d = {[0, 10]: 'primo intervallo'} # ERRORE!!!!

# il tipo tupla è ammissibile per la chiave
d = {(0, 10): 'primo intervallo'}
{% endhighlight %}

#### Accedere agli elementi conservati in un dizionario 

Una volta creato un dizionario, è possibile ottenere il valore associato a una chiave usando la sintassi **nome\_dizionario\[chiave\]**:

{% highlight python %}
# definisco un dizionario di esempio
d = {
    'a': 1, 
    'b': 2, 
    'c': 3, 
    'campana': 'rumorosa', 
    10: 'automobili'
}

d['a'] # ritorna il valore associato alla chiave 'a': 1
d['c'] # ritorna il valore associato alla chiave 'c': 3
d['campana'] # ritorna 'rumorosa'
d[10] # ritorna 'automobili'
{% endhighlight %}

Se viene specificata una chiave inesistente, Python restituisce un *KeyError*. È però possibile usare l’operatore in (o not in) per verificare se una chiave è presente nel dizionario:

{% highlight python %}
d = {'a': 1, 'b': 2, 'c': 3}
d['x'] 

# se la chiave non esiste restituisce un:
# KeyError Traceback (most recent call last):
{% endhighlight %}

#### Operatori in e not in

Gli operatori booleani **in** e **not in** mi permettono di controllare se una specifica chiave è stata definita all’interno di un dizionario.

*Attenzione: questi operatori monitorano la presenza di una chiave ma non monitorano in alcun modo i valori associati alle chiavi stesse.*

{% highlight python %}
if 'x' in d: # la chiave 'x' è presente in d 
    print('Chiave x definita')
    
if 'x' not in d # la chiave 'x' non è presente in d 
    print('Chiave x non definita')
    
if 'b' in d: # la chiave 'b' è presente
    print(d['b']) # il valore associato alla chiave 'b' è 2
{% endhighlight %}

#### Aggiungere e modificare gli elementi di un dizionario

È possibile aggiungere o modificare elementi in un dizionario usando la sintassi **dizionario\[chiave\] = valore**.

{% highlight python %}
# definisco un dizionario che contiene 3 elementi
ilmiodizionario = {'a': 1, 'b': 2, 'c': 3}

# aggiungo un elemento
d['k'] = 2020 
print(d)      # scrive {'a': 1, 'b': 2, 'c': 3, 'k': 2020}

# modifico un elemento 
d['a'] = 123 
print(d)      # scrive {'a': 123, 'b': 2, 'c': 3, 'k': 2020}
{% endhighlight %}

#### Rimuovere un elemento da un dizionario

E’ possibile rimuovere un elemento dal dizionario usando il comando usando la sintassi: **del nome\_dizionario\[chiave\]**:

{% highlight python %}
# definisco un dizionario
d = {'a': 1, 'b': 2, 'c': 3}

# rimuove l'elemento (chiave e valore) con chiave 'a'
del d['a']    
print(d)      # scrive {'b': 2, 'c': 3}
{% endhighlight %}

#### Visita di un dizionario

Visitare un dizionario signigica utilizzare un ciclo per scandire tutti gli elementi che sono al suo interno al fine di fare con questi delle operazioni.

Di solito per visitare un dizionario si utilizza un ciclo for, guardiamo l’esempio in basso:

{% highlight python %}
stati_e_capitali = {
    'Gujarat' : 'Gandhinagar',
    'Maharashtra' : 'Mumbai',
    'Rajasthan' : 'Jaipur',
    'Bihar' : 'Patna'
}

print('Lista degli stati:')

# Visito il dizionario prendendo le sole chiavi
for stato in stati_e_capitali:
    print(stato) # scrive Gujarat, Maharashtra, Rajasthan, Bihar
    
{% endhighlight %}

Nell’esempio mi sono limitato a scrivere (print) le chiavi definite all’interno del dizionario. Avrei però potuto applicare su queste qualsiasi funzione python.

Se volessi utilizzare, all’interno del ciclo, non solo le chiavi ma anche i valori a queste associate utilizzo la notazione che fa uso delle parentesi quadre per accedere al singolo elemento.

{% highlight python %}
print('Lista degli stati e delle relative capitali:')

# Visito la lista prendendo chiavi e valori
# e scrivo sia gli uni sia gli altri
for stato in stati_e_capitali: 
    print(stato, ":", stati_e_capitali[stato])
    
{% endhighlight %}

## Esercizi

#### Esercizio 1:
- definisci un dizionario “persona1” che al suo interno abbia due elementi, il primo con etichetta “nome” e contenuto “Mario”, il secondo con etichetta “cognome” e contenuto “Serenelli”.
- definisci un dizionario “persona2” che al suo interno abbia due elementi, il primo con etichetta “nome” e contenuto “Maria”, il secondo con etichetta “cognome” e contenuto “Giacobini”.
- aggiungi a persona1 l’elemento ad etichetta “indirizzo” con valore “Via Giuseppe Verdi”
- scrivi un ciclo che permetta di scrivere tutto il contenuto di persona1
- scrivi un ciclo che permetta di scrivere tutto il contenuto di persona2
- definisci una lista che contenga persona1 e persona2 appena definiti.

#### Esercizio 2:
Scrivi un algoritmo che concateni due dizionari creandone uno nuovo che contiene tutti gli elementi del primo e tutti gli elementi del secondo.  
dic1={1:10, 2:20}  
dic2={3:30, 4:40}  
Expected Result : {1: 10, 2: 20, 3: 30, 4: 40}

#### Esercizio 3:
Scrivi un programma Python che letto un numero n generi un dizionario i cui elementi abbiano la forma x, x\*x:  
Esempio ( n = 5) :  
Output : {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

#### Esercizio 4:
Scrivi uno script python che controlli se due dizionari hanno le stesse chiavi (tutte le chiavi del primo sono definite nel secondo e viceversa)

#### Esercizio 5:
Scrivi uno script python che ricevuto un dizionario i cui valori sono tutti numeri interi, trovi il massimo valore e il minimo valore.

#### Esercizio 6:
Scrivi un programma Python che controlli se un dizionario è vuoto oppure no.

#### Esercizio 7:
Crea una lista di dizionari che descriva il seguente problema di Knapsack:
m1: peso 23 valore 54
m2: peso 27 valore 59
m3: peso 19 valore 40
m4: peso 26 valore 57

#### Esercizio 8: 

dato il dizionario:
voti = {
  'Fisica': 8,
  'Matematica': 6,
  'Storia': 7
}

Scrivere il nome della disciplina con il voto più basso.
