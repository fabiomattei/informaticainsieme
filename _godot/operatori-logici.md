---
title: 'Godot: operatori logici'
date: '2026-08-18T09:30:00+01:00'
author: Fabio Mattei
layout: page
---

## Operatori logici

Gli operatori logici permettono di **combinare più condizioni** in un'unica
espressione. Il risultato è sempre un valore booleano: `true` o `false`.

GDScript, come Ruby, dispone di tre operatori logici fondamentali: **AND**, **OR** e
**NOT**, ciascuno in due forme equivalenti: simbolica (`&&`, `||`, `!`) e verbale
(`and`, `or`, `not`). Nello stile GDScript ufficiale si preferisce la forma verbale.

---

## AND — entrambe le condizioni devono essere vere

L'operatore `and` (o `&&`) restituisce `true` solo se **entrambe** le condizioni sono
vere.

| A     | B     | A and B |
|-------|-------|---------|
| true  | true  | true    |
| true  | false | false   |
| false | true  | false   |
| false | false | false   |

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var temperatura = 50

    if temperatura > 0 and temperatura <= 100:
        print("Stato liquido")
    elif temperatura <= 0:
        print("Stato solido")
    else:
        print("Stato gassoso")
{% endhighlight %}

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var n = 42
    if n >= 10 and n <= 99:
        print("%d è un numero a due cifre" % n)
    else:
        print("%d non è un numero a due cifre" % n)
{% endhighlight %}

---

## OR — almeno una condizione deve essere vera

L'operatore `or` (o `||`) restituisce `true` se **almeno una** delle due condizioni è
vera.

| A     | B     | A or B |
|-------|-------|--------|
| true  | true  | true   |
| true  | false | true   |
| false | true  | true   |
| false | false | false  |

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var voto = 7
    if voto < 1 or voto > 10:
        print("Voto non valido")
    elif voto >= 6:
        print("Promosso")
    else:
        print("Bocciato")
{% endhighlight %}

---

## NOT — nega il valore della condizione

L'operatore `not` (o `!`) inverte il valore booleano.

| A     | not A |
|-------|-------|
| true  | false |
| false | true  |

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var risposta = "s"

    if not (risposta == "n"):
        print("Accesso consentito")
    else:
        print("Accesso negato")
{% endhighlight %}

GDScript non ha una parola chiave `unless` come Ruby: la condizione negata va sempre
scritta esplicitamente con `not` o `!`, dentro un normale `if`.

---

## Combinare più operatori

Quando si mescolano `and` e `or` nella stessa espressione, `and` viene valutato
**prima** di `or`, esattamente come in Ruby. Per rendere l'intenzione esplicita è
buona norma usare le parentesi.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var a = true
    var b = false
    var c = true

    print(a or b and c)      # true  (b and c prima, poi or a)
    print((a or b) and c)    # true  (a or b prima, poi and c)

    b = true
    c = false

    print(a or b and c)      # true  (b and c = false, poi true or false = true)
    print((a or b) and c)    # false (a or b = true, poi true and false = false)
{% endhighlight %}

---

## Valutazione a corto circuito

Come Ruby, GDScript valuta le condizioni da sinistra a destra e si ferma non appena
il risultato è determinato.

{% highlight gdscript %}
func _ready():
    var n = 0
    if n != 0 and 10 / n > 2:
        print("quoziente maggiore di 2")
    else:
        print("n è zero oppure il quoziente è <= 2")
{% endhighlight %}

Se `n != 0` è falso, GDScript non valuta `10 / n`, evitando la divisione per zero.

---

## Esercizi

#### Esercizio 6
Scrivi uno script che, dato un numero intero, stampi se è positivo, negativo o
nullo.

#### Esercizio 7
Scrivi uno script che, dati tre numeri interi, stampi "tutti uguali" se sono tutti
uguali, "tutti diversi" se sono tutti diversi, altrimenti "né tutti uguali né tutti
diversi".

#### Esercizio 8
Scrivi uno script che, dato un anno, stampi se è bisestile oppure no. Un anno è
bisestile se è divisibile per 4, eccetto i multipli di 100 che non lo sono, a meno
che non siano anche multipli di 400.

#### Esercizio 9
Scrivi uno script che, data una temperatura e l'unità di misura (C o F), indichi se
l'acqua si trova allo stato solido, liquido o gassoso.
