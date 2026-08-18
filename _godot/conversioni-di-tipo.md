---
title: 'Godot: conversioni di tipo'
date: '2026-08-18T09:20:00+01:00'
author: Fabio Mattei
layout: page
---

## Perché convertire i tipi

In GDScript ogni valore ha un tipo. A volte è necessario trasformare un valore da un
tipo a un altro: ad esempio un valore letto da un campo di testo dell'interfaccia
(vedi [raccogliere il testo digitato]({{ site.baseurl }}{% link _godot/testo.md %}.html))
arriva sempre come `String`, anche se rappresenta un numero.

{% highlight gdscript %}
func _ready():
    var testo = "42"
    print(type_string(typeof(testo)))    # String
    var numero = int(testo)
    print(type_string(typeof(numero)))   # int
{% endhighlight %}

A differenza di Ruby, dove le conversioni sono **metodi** chiamati sul valore
(`"42".to_i`), in GDScript sono **funzioni globali** che ricevono il valore come
argomento: `int("42")`.

---

## int() — da stringa o float a intero

`int()` converte il valore in un numero intero. Se il valore è un `float`, la parte
decimale viene **troncata** (non arrotondata). Se il valore è una stringa non
numerica, restituisce 0 senza generare errori.

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    print(int(3.9))       # 3  (troncato, non arrotondato)
    print(int(3.1))       # 3
    print(int("42"))      # 42
    print(int("-7"))      # -7
    print(int("3.14"))    # 3  (si ferma al punto)
    print(int("abc"))     # 0  (nessuna cifra iniziale)
{% endhighlight %}

Per arrotondare invece di troncare si usano le funzioni `round()`, `floor()` e
`ceil()`, che restituiscono comunque un `float`: si combinano con `int()` quando
serve davvero un intero.

{% highlight gdscript %}
func _ready():
    print(round(3.5))    # 4.0
    print(floor(3.9))    # 3.0  (arrotonda sempre per difetto)
    print(ceil(3.1))     # 4.0  (arrotonda sempre per eccesso)
    print(int(round(3.5)))  # 4  (come intero)
{% endhighlight %}

---

## float() — da stringa o intero a numero decimale

`float()` converte il valore in un numero a virgola mobile.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    print(float(5))        # 5.0
    print(float("3.14"))   # 3.14
    print(float("abc"))    # 0.0
{% endhighlight %}

La conversione da intero a float è utile per ottenere la divisione decimale, dato
che in GDScript la divisione tra due interi produce un intero (come in Ruby).

{% highlight gdscript %}
func _ready():
    print(7 / 2)             # 3   (divisione intera)
    print(float(7) / 2)      # 3.5 (divisione decimale)
    print(7 / float(2))      # 3.5
{% endhighlight %}

---

## str() — da numero a stringa

`str()` converte il valore nella sua rappresentazione testuale. È necessario quando
si vuole concatenare un numero a una stringa con `+`.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var eta = 25
    print("Ho " + str(eta) + " anni")   # Ho 25 anni
    print("Pi greco: " + str(3.14))     # Pi greco: 3.14
{% endhighlight %}

In alternativa si può usare l'**operatore %**, che converte automaticamente il
valore in stringa e non richiede `str()` esplicito:

{% highlight gdscript %}
var eta = 25
print("Ho %d anni" % eta)     # Ho 25 anni  (più idiomatico)
{% endhighlight %}

---

## Caratteri e codici Unicode

Ogni carattere è associato a un numero intero nella tabella **Unicode** (che estende
l'ASCII). `char()` chiamato su un intero restituisce il carattere corrispondente;
`.unicode_at(i)` chiamato su una stringa restituisce il codice del carattere in
posizione `i`.

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    print(char(65))    # A
    print(char(97))    # a
    print(char(48))    # 0

    print("A".unicode_at(0))   # 65
    print("a".unicode_at(0))   # 97
    print("0".unicode_at(0))   # 48
{% endhighlight %}

Le lettere maiuscole vanno da 65 (`A`) a 90 (`Z`), le minuscole da 97 (`a`) a 122
(`z`). La differenza tra maiuscola e minuscola è sempre 32.

{% highlight gdscript %}
func _ready():
    print("a".unicode_at(0) - "A".unicode_at(0))     # 32
    print(char("A".unicode_at(0) + 32))               # a  (da maiuscola a minuscola)
    print(char("a".unicode_at(0) - 32))               # A  (da minuscola a maiuscola)
{% endhighlight %}

---

## Valori truthy e falsy

In GDScript le regole di verità sono **diverse da Ruby** e più simili a quelle di
Python: sono **falsy** `null`, `false`, `0`, `0.0`, la stringa vuota `""`, e
collezioni vuote come `[]` e `{}`. Tutto il resto è **truthy**.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire. Osserva la differenza
rispetto a Ruby, dove solo `nil` e `false` sono falsy.

{% highlight gdscript %}
func _ready():
    var valori = [null, false, 0, "", [], 0.0]
    for val in valori:
        if val:
            print(str(val) + " è truthy")
        else:
            print(str(val) + " è falsy")
{% endhighlight %}

Output atteso: tutti i valori dell'array risultano **falsy** — l'opposto di Ruby, dove
`0`, `""` e `[]` sono truthy.

---

## Tabella riepilogativa

| Funzione     | Converte a  | Note                                        |
|--------------|-------------|-----------------------------------------------|
| `int(x)`     | `int`       | Tronca i decimali; legge le cifre iniziali     |
| `float(x)`   | `float`     | Accetta stringhe decimali                      |
| `str(x)`     | `String`    | L'operatore `%` lo chiama automaticamente      |
| `bool(x)`    | `bool`      | Segue le regole di verità viste sopra          |
| `char(x)`    | `String`    | Codice Unicode → carattere                     |
| `.unicode_at(i)` | `int`   | Carattere → codice Unicode                     |
| `round/floor/ceil` | `float` | Arrotondano un numero decimale             |

---

## Esercizi

#### Esercizio 6
Scrivi uno script con due variabili numeriche intere e stampa la loro somma,
differenza, prodotto e quoziente (con decimali).

#### Esercizio 7
Scrivi uno script che, data una variabile con un numero decimale, stampi
separatamente la parte intera e la parte decimale.

#### Esercizio 8
Scrivi uno script che, data una variabile con un carattere minuscolo, stampi il suo
codice Unicode e la corrispondente lettera maiuscola, senza usare `.to_upper()`
(usa `.unicode_at()` e `char()`).

#### Esercizio 9
Scrivi uno script che stampi la tabella dei caratteri stampabili da codice 65 a 90
(le lettere maiuscole), nel formato `65  A`.
