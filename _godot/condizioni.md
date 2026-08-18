---
title: 'Godot: condizioni'
date: '2026-08-18T10:10:00+01:00'
author: Fabio Mattei
layout: page
---

## Il costrutto if

Il costrutto **if** consente di cambiare la sequenza logica di istruzioni da
eseguire in un programma: il blocco di codice che segue viene eseguito solo se la
condizione risulta vera.

A differenza di Ruby, che usa `if ... end`, GDScript segue lo stile di Python:
**niente parola chiave di chiusura**, il blocco è delimitato dall'**indentazione**
(sempre con gli stessi spazi o sempre con tab, senza mischiarli) e la riga della
condizione termina con i due punti `:`.

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var a = 33
    var b = 200
    if b > a:
        print("b è maggiore di a")
{% endhighlight %}

In GDScript **l'indentazione non è solo una questione di leggibilità come in Ruby:
è obbligatoria** e determina quali istruzioni appartengono al blocco. Un errore di
indentazione è un errore di sintassi.

### Operatori di confronto

| Operatore | Significato       | Esempio  |
|-----------|--------------------|----------|
| `==`      | uguale             | `a == b` |
| `!=`      | diverso            | `a != b` |
| `<`       | minore             | `a < b`  |
| `<=`      | minore o uguale    | `a <= b` |
| `>`       | maggiore           | `a > b`  |
| `>=`      | maggiore o uguale  | `a >= b` |

---

## Il costrutto elif

`elif` (non `elsif` come in Ruby) permette di aggiungere una condizione alternativa
quando la prima non è verificata.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var a = 33
    var b = 33
    if b > a:
        print("b è più grande di a")
    elif a == b:
        print("a e b sono uguali")
{% endhighlight %}

---

## Il costrutto else

Se nessuna delle condizioni precedenti è verificata, viene eseguito il blocco
`else`.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var a = 33
    var b = 33
    if b > a:
        print("b è più grande di a")
    elif a == b:
        print("a e b sono uguali")
    else:
        print("a è più grande di b")
{% endhighlight %}

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var n = -4
    if n > 0:
        print("il numero è positivo")
    elif n < 0:
        print("il numero è negativo")
    else:
        print("il numero è zero")
{% endhighlight %}

---

## Niente unless

GDScript **non ha** una parola chiave `unless` come Ruby: la condizione negata va
sempre scritta con `not` (o `!`) dentro un normale `if`, come visto nella pagina
sugli [operatori logici]({{ site.baseurl }}{% link _godot/operatori-logici.md %}.html).

{% highlight gdscript %}
func _ready():
    var risposta = "s"
    if not (risposta == "n"):
        print("Benvenuto!")
    else:
        print("Accesso negato.")
{% endhighlight %}

---

## Il costrutto match

`match` è l'equivalente GDScript del `case/when` di Ruby: confronta un valore con
più alternative in modo più compatto rispetto a una catena di `elif`.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var voto = 8
    match voto:
        0, 1, 2, 3, 4, 5:
            print("insufficiente")
        6:
            print("sufficiente")
        7:
            print("discreto")
        8:
            print("buono")
        9, 10:
            print("ottimo")
        _:
            print("voto non valido")
{% endhighlight %}

Il simbolo `_` (underscore) è il **jolly**: corrisponde a qualsiasi valore non
ancora intercettato dalle clausole precedenti, esattamente come `else` in un
`case/when`.

#### Esercizio 6
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var mese = 7
    match mese:
        3, 4, 5:
            print("primavera")
        6, 7, 8:
            print("estate")
        9, 10, 11:
            print("autunno")
        12, 1, 2:
            print("inverno")
        _:
            print("mese non valido")
{% endhighlight %}

A differenza del `case/when` di Ruby, `match` di GDScript **non supporta i
range direttamente** nelle clausole (non si può scrivere `0..5:`): per gli
intervalli numerici resta più adatta una catena di `if/elif`, come già visto nella
pagina su [range()]({{ site.baseurl }}{% link _godot/range.md %}.html).

---

## Esercizi

#### Esercizio 7
Scrivi uno script che, dati due numeri, stampi il più grande tra i due.

#### Esercizio 8
Scrivi uno script che, dato un numero intero, determini se è pari o dispari
(operatore `%`).

#### Esercizio 9
Scrivi uno script che, dati i tre lati di un triangolo, determini se è scaleno,
isoscele o equilatero.

#### Esercizio 10
Scrivi uno script che, data una spesa, calcoli lo sconto secondo la tabella:

| Spesa                | Sconto         |
|------------------------|------------------|
| Al di sotto di 100 €   | nessuno sconto   |
| Tra 100 e 300 €        | sconto del 10%   |
| Tra 300 e 500 €        | sconto del 15%   |
| Tra 500 e 800 €        | sconto del 20%   |

#### Esercizio 11
Scrivi uno script che, dato un mese (1-12) usando `match`, stampi la stagione
corrispondente.
