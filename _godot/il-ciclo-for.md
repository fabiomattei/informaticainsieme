---
title: 'Godot: il ciclo for'
date: '2026-08-18T10:25:00+01:00'
author: Fabio Mattei
layout: page
---

## Il ciclo for

Il ciclo `for` itera su un array, un dizionario, una stringa o il risultato di
`range()`, assegnando a ogni iterazione il valore corrente a una variabile di
ciclo. Come in Ruby, non richiede di gestire manualmente un contatore.

{% highlight gdscript %}
for <variabile> in <collezione>:
    <istruzione 1>
    <istruzione 2>
{% endhighlight %}

## for su range()

GDScript non ha l'operatore `..`/`...` di Ruby: al suo posto si usa la funzione
`range()`, vista in dettaglio nella pagina su [range()]({{ site.baseurl }}{% link _godot/range.md %}.html).

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    for i in range(1, 6):
        print(i)
{% endhighlight %}

Output: 1, 2, 3, 4, 5 — il limite superiore di `range()` è sempre escluso.

| Iterazione | i |
|:----------:|:-:|
| 1          | 1 |
| 2          | 2 |
| 3          | 3 |
| 4          | 4 |
| 5          | 5 |

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire. Osserva come si calcola
la somma dei numeri da 1 a 100 con una variabile accumulatore.

{% highlight gdscript %}
func _ready():
    var acc = 0
    for i in range(1, 101):
        acc += i
    print(acc)    # 5050
{% endhighlight %}

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire. Questo programma
stampa i numeri pari da 2 a 20.

{% highlight gdscript %}
func _ready():
    for i in range(1, 21):
        if i % 2 == 0:
            print(i)
{% endhighlight %}

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire. Osserva come si
costruisce e si stampa la tavola pitagorica di un numero.

{% highlight gdscript %}
func _ready():
    var n = 7
    for i in range(1, 11):
        print("%d x %d = %d" % [n, i, n * i])
{% endhighlight %}

---

## for su un array

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    for i in [1, 3, 5, 7, 9]:
        print(i)
{% endhighlight %}

| Iterazione | i |
|:----------:|:-:|
| 1          | 1 |
| 2          | 3 |
| 3          | 5 |
| 4          | 7 |
| 5          | 9 |

#### Esercizio 6
Copia il seguente codice dentro `_ready()` e fallo eseguire. Questo programma trova
il valore più grande in un array usando la variabile **campione**.

{% highlight gdscript %}
func _ready():
    var numeri = [34, 7, 89, 12, 55, 3, 78]
    var massimo = numeri[0]
    for n in numeri:
        if n > massimo:
            massimo = n
    print("Il massimo è: %d" % massimo)
{% endhighlight %}

---

## for su un dizionario

Quando si itera su un `Dictionary` con `for`, la variabile di ciclo scorre le
**chiavi**: il valore va recuperato separatamente.

#### Esercizio 7
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var persona = {"nome": "Mario", "eta": 25, "citta": "Roma"}
    for chiave in persona:
        print("%s: %s" % [chiave, persona[chiave]])
{% endhighlight %}

#### Esercizio 8
Copia il seguente codice dentro `_ready()` e fallo eseguire. Questo programma
calcola il totale dei prezzi di un piccolo magazzino.

{% highlight gdscript %}
func _ready():
    var magazzino = {"mele": 1.20, "pane": 0.80, "latte": 1.50, "pasta": 0.90}
    var totale = 0.0
    for prodotto in magazzino:
        print("%s: %s €" % [prodotto, magazzino[prodotto]])
        totale += magazzino[prodotto]
    print("Totale: %s €" % totale)
{% endhighlight %}

---

## Controllo del flusso: continue e break

All'interno di un ciclo si possono usare due istruzioni speciali. GDScript non ha
`redo` (che in Ruby ripete l'iterazione corrente): esistono solo `continue` e
`break`.

| Istruzione | Effetto                                         |
|------------|---------------------------------------------------|
| `continue` | Salta immediatamente all'iterazione successiva     |
| `break`    | Esce immediatamente dal ciclo                      |

#### Esercizio 9
Copia il seguente codice dentro `_ready()` e fallo eseguire. Osserva quali numeri
vengono stampati e perché.

{% highlight gdscript %}
func _ready():
    for i in range(1, 11):
        if i == 3:
            continue
        if i == 7:
            break
        print(i)
{% endhighlight %}

Il programma salta il numero 3 e interrompe il ciclo quando `i` vale 7. L'output è:
1, 2, 4, 5, 6.

#### Esercizio 10
Copia il seguente codice dentro `_ready()` e fallo eseguire. `continue` viene usato
per saltare i numeri dispari e sommare solo i pari.

{% highlight gdscript %}
func _ready():
    var acc = 0
    for i in range(1, 21):
        if i % 2 != 0:
            continue
        acc += i
    print("Somma dei pari da 1 a 20: %d" % acc)
{% endhighlight %}

---

## Cicli annidati

È possibile inserire un ciclo `for` all'interno di un altro. Il ciclo interno viene
eseguito completamente a ogni iterazione del ciclo esterno.

#### Esercizio 11
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    for riga in range(1, 4):
        for col in range(1, 4):
            printraw("%d,%d  " % [riga, col])
        print("")
{% endhighlight %}

#### Esercizio 12
Copia il seguente codice dentro `_ready()` e fallo eseguire. Questo programma
stampa la tabella della moltiplicazione completa.

{% highlight gdscript %}
func _ready():
    for i in range(1, 11):
        var riga = ""
        for j in range(1, 11):
            riga += "%4d" % (i * j)
        print(riga)
{% endhighlight %}

---

## Esercizi

#### Esercizio 13
Scrivi uno script che, dato un numero intero N, trovi tutti i suoi divisori.

#### Esercizio 14
Scrivi uno script che trovi tutti i numeri primi compresi tra 1 e 100.

#### Esercizio 15
Scrivi uno script che utilizzi due cicli `for` annidati per scrivere la tabella
dell'addizione completa (da 1+1 a 10+10).

#### Esercizio 16
Scrivi uno script che, dato un numero intero N, stampi un triangolo rettangolo di
asterischi alto N righe.

#### Esercizio 17
Scrivi uno script che, dato un numero intero N, verifichi se è primo (un numero è
primo se ha esattamente due divisori: 1 e se stesso).
