---
title: 'Godot: strutture dati leggere'
date: '2026-08-18T09:55:00+01:00'
author: Fabio Mattei
layout: page
---

## Niente Struct nativo

Ruby offre `Struct.new(:x, :y)` come scorciatoia per creare rapidamente una classe
semplice con campi nominati, senza scrivere `initialize` a mano. GDScript **non ha
un equivalente diretto di Struct**: la soluzione più vicina è definire una piccola
**classe interna** (una `class` dentro lo script), che in GDScript si scrive quasi
con la stessa brevità.

---

## Una classe interna come "struct"

#### Esercizio 1
Copia il seguente codice dentro `_ready()`, insieme alla definizione della classe
(fuori da `_ready()`, a livello di script), e fallo eseguire.

{% highlight gdscript %}
class Punto:
    var x
    var y

    func _init(nuovo_x, nuovo_y):
        x = nuovo_x
        y = nuovo_y

func _ready():
    var p1 = Punto.new(3, 4)
    print(p1.x)    # 3
    print(p1.y)    # 4
{% endhighlight %}

`class Punto:` definisce una classe **interna** allo script corrente. `_init()` è il
costruttore: viene chiamato automaticamente quando si scrive `Punto.new(3, 4)`, ed è
l'equivalente dell'`initialize` di Ruby.

---

## Aggiungere metodi

Come in una classe normale, si possono aggiungere metodi che operano sui campi.

#### Esercizio 2
Copia il seguente codice dentro lo script (classe fuori da `_ready()`, chiamata
dentro) e fallo eseguire.

{% highlight gdscript %}
class Punto:
    var x
    var y

    func _init(nuovo_x, nuovo_y):
        x = nuovo_x
        y = nuovo_y

    func distanza_dall_origine():
        return sqrt(pow(x, 2) + pow(y, 2))

    func to_s():
        return "(%s, %s)" % [x, y]

func _ready():
    var p1 = Punto.new(3, 4)
    print(p1.to_s())                     # (3, 4)
    print(p1.distanza_dall_origine())    # 5.0
{% endhighlight %}

A differenza di Ruby, dove `Struct.new` genera automaticamente `to_s` e
l'accesso ai campi (`p1.x`), in GDScript i campi sono già direttamente accessibili
(`p1.x`, `p1.y`) perché `var` all'interno di una classe crea sempre un attributo
pubblico: non serve nessun `attr_accessor`.

---

## Struct in array

#### Esercizio 3
Copia il seguente codice dentro lo script e fallo eseguire.

{% highlight gdscript %}
class Studente:
    var nome
    var voto

    func _init(n, v):
        nome = n
        voto = v

func _ready():
    var classe = [
        Studente.new("Alice", 9),
        Studente.new("Bruno", 6),
        Studente.new("Carla", 8),
        Studente.new("Davide", 7)
    ]

    for studente in classe:
        print("%s: %d" % [studente.nome, studente.voto])

    var promossi = []
    for studente in classe:
        if studente.voto >= 6:
            promossi.append(studente.nome)
    print("Promossi: %s" % [promossi])
{% endhighlight %}

---

## Confrontare le istanze

A differenza dello Struct di Ruby, che confronta automaticamente **per valore**
(campo per campo), due istanze di classe in GDScript sono uguali con `==` solo se
sono **lo stesso oggetto** in memoria — proprio come le classi normali di Ruby.

{% highlight gdscript %}
class Punto:
    var x
    var y
    func _init(nuovo_x, nuovo_y):
        x = nuovo_x
        y = nuovo_y

func _ready():
    var a = Punto.new(1, 2)
    var b = Punto.new(1, 2)

    print(a == b)    # false — sono due oggetti diversi, anche se i campi coincidono
    print(a.x == b.x and a.y == b.y)    # true — confronto manuale campo per campo
{% endhighlight %}

---

## Quando usare una classe interna

| Caratteristica            | Classe interna GDScript | Dictionary          |
|-----------------------------|--------------------------|----------------------|
| Campi nominati fissi        | sì (ma non imposti)      | no                   |
| Costruttore                 | sì, con `_init`          | —                    |
| Metodi personalizzati       | sì                       | no                   |
| Confronto per valore        | no (di default)          | non applicabile      |
| Verbosità                   | media                    | bassa                |

Negli esempi di gioco di questa sezione useremo più spesso semplici `Dictionary`
(come già visto nella pagina sui [Dizionari]({{ site.baseurl }}{% link _godot/dizionari.md %}.html))
per rappresentare oggetti come una pallina o un nemico, perché sono più rapidi da
scrivere; una classe interna diventa preferibile quando servono metodi propri, come
vedremo nella pagina sulle [classi]({{ site.baseurl }}{% link _godot/classi.md %}.html).

---

## Esercizi

#### Esercizio 4
Definisci una classe interna `Rettangolo` con i campi `base` e `altezza`. Aggiungi i
metodi `area()` e `perimetro()`. Crea tre istanze e stampa area e perimetro di
ciascuna.

#### Esercizio 5
Definisci una classe interna `Prodotto` con `nome`, `prezzo` e `quantita`. Crea un
array di almeno 5 prodotti e stampa: il prodotto più costoso, il valore totale in
magazzino, e tutti i prodotti con prezzo inferiore a 10.

#### Esercizio 6
Definisci una classe interna `Contatto` con `nome`, `telefono` ed `email`. Simula una
rubrica: crea 4 contatti in un array e scrivi uno script che, dato un nome fissato in
una variabile, stampi i dati del contatto corrispondente (o "Non trovato").
