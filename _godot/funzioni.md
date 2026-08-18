---
title: 'Godot: funzioni'
date: '2026-08-18T10:30:00+01:00'
author: Fabio Mattei
layout: page
---

## Funzioni

In GDScript una funzione si definisce con la parola chiave `func`, seguita dai due
punti e da un blocco indentato — non c'è un `end` come in Ruby. Le funzioni
raggruppano una sequenza di istruzioni che può essere richiamata più volte dal
resto del programma.

#### Esercizio 1
Copia il seguente codice dentro lo script (la funzione va scritta fuori da
`_ready()`, a livello di script) e fallo eseguire.

{% highlight gdscript %}
func saluta():
    print("Ciao!")
    print("Ciao!!!")
    print("Ciao a te!")

func _ready():
    print("Prima della chiamata")
    saluta()
    saluta()
    print("Dopo le chiamate")
{% endhighlight %}

## Parametri e argomenti

Una funzione può ricevere valori dall'esterno attraverso i **parametri**. A
differenza di Ruby, in GDScript le **parentesi sono sempre obbligatorie**, anche
quando la funzione non ha parametri (`func saluta():`) o ne ha uno solo.

#### Esercizio 2
Copia il seguente codice nello script e fallo eseguire.

{% highlight gdscript %}
func saluta(nome):
    print("Ciao " + nome)

func _ready():
    saluta("Alice")
    saluta("Bob")
{% endhighlight %}

---

## Valori di ritorno

Una funzione restituisce un valore al chiamante con l'istruzione `return`.

#### Esercizio 3
Copia il seguente codice nello script e fallo eseguire.

{% highlight gdscript %}
func raddoppia(numero):
    return numero * 2

func _ready():
    var risultato = raddoppia(5)
    print(risultato)
    print(raddoppia(10))
{% endhighlight %}

### Niente return implicito

A differenza di Ruby, dove l'ultima espressione valutata diventa automaticamente il
valore restituito, **in GDScript `return` è sempre obbligatorio** se si vuole
restituire un valore: senza `return` esplicito, una funzione restituisce sempre
`null`.

#### Esercizio 4
Copia il seguente codice nello script e fallo eseguire. Osserva cosa succede senza
`return`.

{% highlight gdscript %}
func raddoppia_sbagliata(numero):
    numero * 2    # il risultato viene calcolato ma non restituito!

func _ready():
    print(raddoppia_sbagliata(5))    # null — non 10!
{% endhighlight %}

#### Esercizio 5
Copia il seguente codice nello script e fallo eseguire. Osserva come `return` esca
dalla funzione immediatamente: le istruzioni successive non vengono eseguite.

{% highlight gdscript %}
func pari_o_dispari(numero):
    if numero % 2 == 0:
        return "pari"
    return "dispari"

func _ready():
    print(pari_o_dispari(4))
    print(pari_o_dispari(7))
{% endhighlight %}

---

## Parametri facoltativi

Un parametro con valore di default diventa **facoltativo**: se il chiamante non lo
passa, viene usato il valore di default. Come in Ruby, i parametri facoltativi
devono seguire quelli obbligatori.

#### Esercizio 6
Copia il seguente codice nello script e fallo eseguire.

{% highlight gdscript %}
func moltiplica(a, b, c = 2):
    return a * b * c

func _ready():
    print(moltiplica(3, 4))       # c vale 2 per default
    print(moltiplica(3, 4, 5))    # c vale 5
{% endhighlight %}

---

## Niente splat: parametri a numero variabile

Ruby permette di raccogliere un numero variabile di argomenti con `*args`.
**GDScript non ha questa possibilità per le funzioni definite dall'utente**: il
numero di parametri è sempre fisso. Per simulare lo stesso effetto si passa
esplicitamente un `Array`.

#### Esercizio 7
Copia il seguente codice nello script e fallo eseguire.

{% highlight gdscript %}
func somma_tutto(numeri):
    var acc = 0
    for n in numeri:
        acc += n
    return acc

func _ready():
    print(somma_tutto([1, 2, 3]))
    print(somma_tutto([10, 20, 30, 40]))
{% endhighlight %}

Invece di `somma_tutto(1, 2, 3)` come si scriverebbe in Ruby con lo splat, in
GDScript si scrive `somma_tutto([1, 2, 3])`, passando l'array esplicitamente.

---

## Tipizzare parametri e valore di ritorno

Come per le variabili (vedi la pagina sulle [variabili]({{ site.baseurl }}{% link _godot/variabili.md %}.html)),
GDScript permette di dichiarare esplicitamente i tipi di parametri e valore di
ritorno: è opzionale, ma rende l'editor capace di segnalare subito un errore se si
chiama la funzione con l'argomento sbagliato.

{% highlight gdscript %}
func somma(a: int, b: int) -> int:
    return a + b

func _ready():
    print(somma(3, 4))    # 7
{% endhighlight %}

`-> int` dopo la lista dei parametri dichiara il tipo del valore restituito.

---

## Ambito delle variabili (scope)

Le variabili definite all'interno di una funzione sono **locali**: nascono quando
la funzione viene chiamata e vengono distrutte quando termina, esattamente come in
Ruby. Approfondiamo questo argomento nella pagina sulla
[visibilità delle variabili]({{ site.baseurl }}{% link _godot/visibilita-variabili.md %}.html).

{% highlight gdscript %}
func pollaio():
    var uova = 32765
    print(uova)

func _ready():
    pollaio()
    # print(uova)    # errore: uova non esiste qui fuori
{% endhighlight %}

---

## Esercizi

#### Esercizio 8
Scrivi una funzione `calcola_maggiore` che prenda due numeri come parametri e
restituisca il più grande tra i due.

#### Esercizio 9
Scrivi una funzione `fattoriale` che accetti un numero intero e restituisca il suo
fattoriale (1 × 2 × 3 × … × N).

#### Esercizio 10
Scrivi una funzione `e_primo` che accetti un numero intero e restituisca `true` se è
primo e `false` altrimenti.

#### Esercizio 11
Scrivi una funzione `mcd` che calcoli il massimo comune divisore tra due numeri
usando l'algoritmo di Euclide.

#### Esercizio 12
Scrivi una funzione `celsius_a_fahrenheit` che converta una temperatura da gradi
Celsius a Fahrenheit (formula: `Tf = Tc * 9.0 / 5 + 32`). Scrivi poi la funzione
inversa `fahrenheit_a_celsius`.

#### Esercizio 13
Scrivi una funzione `calcola_stipendio` che prenda come parametri il numero di ore
lavorate e la paga oraria. Fino a 40 ore lo stipendio è `ore * paga`; per le ore di
straordinario si applica la paga maggiorata del 50%.
