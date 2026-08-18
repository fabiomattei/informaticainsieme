---
title: 'Godot: variabili'
date: '2026-08-18T09:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Come provare gli esempi di questa pagina

Per tutti gli esempi di questa sezione useremo lo stesso schema: apri Godot, crea una
nuova scena con un singolo nodo `Node`, clicca sull'icona dello script per crearne uno
nuovo (linguaggio **GDScript**), e scrivi il codice dentro la funzione `_ready()`, che
Godot esegue automaticamente una sola volta quando la scena parte. Premi **F6** per
eseguire la scena corrente e osserva il pannello **Output** in basso: è lì che compaiono
i risultati di `print()`.

{% highlight gdscript %}
extends Node

func _ready():
    print("Ciao 4C!")
{% endhighlight %}

## Contenitori di informazioni

Le variabili sono **contenitori di informazioni**. Possiamo pensare a una variabile
come a un cassetto dotato di etichetta: ogni variabile ha un **nome** che serve per
ritrovarla facilmente, e un **valore** che è l'informazione conservata al suo interno.

Per creare una variabile in GDScript si usa la parola chiave `var`:

{% highlight gdscript %}
var x = 5
var y = "Mario"
var z = 3.14
{% endhighlight %}

A differenza di Ruby, in GDScript **è obbligatorio** scrivere `var` prima del nome della
variabile la prima volta che la si usa. Questo aiuta l'editor a segnalare subito un
errore di battitura nel nome, invece di crearne una nuova per sbaglio.

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var numero_panini = 6
    var costo_panino = 2
    var costo_totale = numero_panini * costo_panino
    print(costo_totale)
{% endhighlight %}

Osserva che:
- alle variabili sono stati dati nomi significativi
- nelle variabili si memorizzano valori ma non le unità di misura
- il simbolo `*` indica la moltiplicazione

---

## Nomi di variabile

In GDScript il nome di una variabile segue queste regole:

- deve iniziare con una **lettera** o con il trattino basso `_`
- non può iniziare con un numero
- può contenere lettere, numeri e trattino basso (`a-z`, `0-9`, `_`)
- i nomi sono **case-sensitive**: `volume`, `Volume` e `VOLUME` sono tre variabili
  distinte

La convenzione GDScript, come in Ruby, è lo **snake_case**: parole separate da
trattino basso, tutte in minuscolo.

{% highlight gdscript %}
var nome = "Alice"
var cognome = "Rossi"
var eta = 17
var voto_medio = 8.5
var numero_di_studenti = 25
{% endhighlight %}

---

## Tipizzazione statica opzionale

GDScript, come Ruby, non richiede di dichiarare il tipo di una variabile: viene dedotto
automaticamente dal valore assegnato. A differenza di Ruby, però, GDScript permette
anche di **dichiarare esplicitamente il tipo**, se lo si desidera, mettendolo dopo i
due punti: questo aiuta l'editor a segnalare errori prima ancora di eseguire il codice.

{% highlight gdscript %}
var eta: int = 17           # tipizzata esplicitamente
var nome := "Alice"         # tipizzata per inferenza, con :=
var altezza = 1.70          # non tipizzata (dinamica, come in Ruby)
{% endhighlight %}

Le tre forme convivono nello stesso script: si può scegliere quella più adatta caso
per caso. Negli esempi di questa sezione useremo quasi sempre la forma dinamica
`var nome = valore`, la più simile a quella già vista in Ruby.

---

## Operatori di assegnazione composti

GDScript offre gli stessi operatori abbreviati visti in Ruby per le operazioni più
comuni sulla stessa variabile.

| Operatore | Equivalente a | Esempio  |
|-----------|---------------|----------|
| `+=`      | `a = a + n`   | `x += 3` |
| `-=`      | `a = a - n`   | `x -= 3` |
| `*=`      | `a = a * n`   | `x *= 2` |
| `/=`      | `a = a / n`   | `x /= 2` |
| `%=`      | `a = a % n`   | `x %= 3` |

GDScript **non** ha un operatore `**` per la potenza: per elevare a potenza si usa la
funzione `pow(base, esponente)`.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var punteggio = 100
    punteggio += 50    # 150
    punteggio -= 20     # 130
    punteggio *= 2       # 260
    print(punteggio)
{% endhighlight %}

---

## Assegnazione multipla

A differenza di Ruby, GDScript **non** permette di assegnare più variabili in una sola
istruzione (`a, b, c = 10, 20, 30` non è valido). Ogni variabile va assegnata sulla sua
riga.

{% highlight gdscript %}
var a = 10
var b = 20
var c = 30
print(a)   # 10
print(b)   # 20
print(c)   # 30
{% endhighlight %}

## Scambiare due variabili

Senza assegnazione parallela, per scambiare due variabili serve una **variabile
temporanea**.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var a = "primo"
    var b = "secondo"

    print("Prima: a=%s, b=%s" % [a, b])
    var temp = a
    a = b
    b = temp
    print("Dopo:  a=%s, b=%s" % [a, b])
{% endhighlight %}

---

## Il tipo di una variabile

In GDScript la funzione `typeof()` restituisce un codice numerico che rappresenta il
tipo del valore; `type_string()` lo trasforma in un nome leggibile.

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var x = 42
    print(type_string(typeof(x)))      # int

    x = 3.14
    print(type_string(typeof(x)))      # float

    x = "ciao"
    print(type_string(typeof(x)))      # String

    x = true
    print(type_string(typeof(x)))      # bool
{% endhighlight %}

La stessa variabile può contenere valori di tipo diverso in momenti diversi: come in
Ruby, il tipo è legato al **valore**, non alla variabile — a meno che la variabile non
sia stata dichiarata con un tipo esplicito, nel qual caso Godot impedisce di assegnarle
un valore di tipo diverso.

---

## Tabella di tracciamento

Quando un programma modifica le variabili in sequenza, è utile disegnare una
**tabella di tracciamento** per seguire i cambiamenti.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire, poi compila la tabella
di tracciamento.

{% highlight gdscript %}
func _ready():
    var a = 3
    var b = 5
    var c = a + b
    a = c * 2
    b = a - c
    print(a)
    print(b)
    print(c)
{% endhighlight %}

| Istruzione   | a  | b  | c  |
|--------------|----|----|----|
| `a = 3`      | 3  | —  | —  |
| `b = 5`      | 3  | 5  | —  |
| `c = a + b`  | 3  | 5  | 8  |
| `a = c * 2`  | 16 | 5  | 8  |
| `b = a - c`  | 16 | 8  | 8  |

---

## Esercizi

#### Esercizio 6
Scrivi uno script che memorizzi in tre variabili i lati di un triangolo rettangolo
(base, altezza e ipotenusa) e stampi il perimetro.

#### Esercizio 7
Un'automobile percorre 120 km con 8 litri di carburante. Scrivi uno script che calcoli
e stampi i km percorsi per litro e quanti litri servono per percorrere 500 km.

#### Esercizio 8
Scrivi uno script con variabili per nome, cognome e anno di nascita, e stampi una
frase del tipo: `"Mario Rossi ha 25 anni."` (calcola l'età dall'anno corrente 2026).

#### Esercizio 9
Scrivi uno script che, date base e altezza di un rettangolo, calcoli area e perimetro.

#### Esercizio 10
Scrivi uno script che, dati tre numeri, ne stampi la somma, il prodotto e la media.
