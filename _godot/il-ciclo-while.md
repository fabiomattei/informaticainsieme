---
title: 'Godot: il ciclo while'
date: '2026-08-18T10:15:00+01:00'
author: Fabio Mattei
layout: page
---

## Il ciclo while

Il costrutto `while` permette di eseguire un blocco di istruzioni più volte,
esattamente come in Ruby. La sintassi segue lo stile GDScript: due punti al posto
di `end`, e il blocco delimitato dall'indentazione.

{% highlight gdscript %}
while <condizione>:
    <istruzione 1>
    <istruzione 2>
{% endhighlight %}

`while` è un ciclo con **controllo in testa**: la condizione viene verificata
**prima** di entrare nel ciclo. Se è già falsa all'inizio, il blocco non viene mai
eseguito.

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var cont = 0
    while cont < 10:
        cont += 1
        print(cont)
{% endhighlight %}

La variabile `cont` si chiama **contatore** perché il suo scopo è contare le
iterazioni.

| Iterazione | cont (prima) | cont (dopo) | Condizione cont < 10 |
|:----------:|:------------:|:-----------:|:---------------------:|
| 1          | 0            | 1           | vera                  |
| 2          | 1            | 2           | vera                  |
| …          | …            | …           | …                     |
| 10         | 9            | 10          | vera                  |
| —          | 10           | —           | **falsa** → uscita    |

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire. Osserva come la
variabile `acc` accumula la somma dei numeri da 1 a 10.

{% highlight gdscript %}
func _ready():
    var n = 10
    var cont = 1
    var acc = 0
    while cont <= n:
        acc += cont
        cont += 1
    print(acc)
{% endhighlight %}

La coppia contatore/accumulatore è uno degli schemi più ricorrenti nella
programmazione, identico a quello già visto in Ruby.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire. Questo programma
calcola la sequenza di Fibonacci: ogni numero è la somma dei due precedenti.

{% highlight gdscript %}
func _ready():
    var acc_a = 0
    var acc_b = 1
    var cont = 0
    while cont < 20:
        printraw(str(acc_a) + " ")
        var nuovo = acc_a + acc_b
        acc_a = acc_b
        acc_b = nuovo
        cont += 1
    print("")
{% endhighlight %}

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire. Osserva come la
variabile `massimo` conserva il valore più grande incontrato finora, scorrendo un
array invece di leggere l'input da tastiera (che vedremo più avanti nella sezione
dedicata al gioco).

{% highlight gdscript %}
func _ready():
    var numeri = [12, 45, 3, 67, 21, 8]
    var massimo = null
    var cont = 0
    while cont < numeri.size():
        var num = numeri[cont]
        if massimo == null or num > massimo:
            massimo = num
        cont += 1
    print("Il massimo è: %d" % massimo)
{% endhighlight %}

La variabile `massimo` si chiama **campione**: a ogni iterazione confronta il nuovo
valore con quello migliore trovato finora e lo aggiorna se necessario.

---

## Niente begin...end while

Ruby offre `begin ... end while`, un ciclo che esegue il corpo **almeno una volta**
prima di valutare la condizione (do-while). GDScript **non ha un costrutto
equivalente**: per ottenere lo stesso effetto si esegue il corpo una prima volta
fuori dal ciclo, e poi si ripete dentro un `while` normale.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire. Il ciclo continua a
sommare numeri di un array finché non incontra uno zero, simulando l'idea di
"leggi finché non arriva lo zero" già vista in Ruby.

{% highlight gdscript %}
func _ready():
    var valori = [5, 8, 3, 0, 9]
    var indice = 0
    var totale = 0
    var n = valori[indice]
    while n != 0:
        totale += n
        indice += 1
        n = valori[indice]
    print("Totale: %d" % totale)
{% endhighlight %}

Da notare la riga `var n = valori[indice]` ripetuta due volte: una prima del ciclo
(per avere un primo valore da controllare) e una dentro il ciclo (per leggere il
valore successivo) — è esattamente la duplicazione che in Ruby si evita con
`begin ... end while`.

---

## break e continue

All'interno di un ciclo `while` si possono usare `break` (esce subito dal ciclo) e
`continue` (salta alla prossima iterazione), gli stessi visti anche nel ciclo `for`.

#### Esercizio 6
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var cont = 0
    while true:
        cont += 1
        if cont == 3:
            continue
        if cont > 6:
            break
        print(cont)
{% endhighlight %}

`while true:` crea un ciclo che andrebbe avanti all'infinito: è `break` a doverlo
interrompere. Questo schema è comune nel game loop di un vero videogioco, che
vedremo nella pagina sull'[interfaccia e il primo script]({{ site.baseurl }}{% link _godot/introduzione.md %}.html).

---

## Esercizi

#### Esercizio 7
Scrivi uno script che, dato un numero intero N, calcoli con un `while` la somma di
tutti i numeri da 1 a N.

#### Esercizio 8
Scrivi uno script che, dato un numero intero N, calcoli il fattoriale di N
(1 × 2 × 3 × … × N) con un `while`.

#### Esercizio 9
Scrivi uno script che, dato un array di numeri, calcoli il massimo e il minimo tra i
valori con un `while`.

#### Esercizio 10
Scrivi uno script che, dato un numero intero positivo N, calcoli la somma di tutte
le sue cifre (usa `%` per estrarre l'ultima cifra e la divisione intera per
eliminarla).
