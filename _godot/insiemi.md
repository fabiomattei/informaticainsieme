---
title: 'Godot: gli insiemi'
date: '2026-08-18T09:50:00+01:00'
author: Fabio Mattei
layout: page
---

## Niente Set nativo

Ruby offre la classe `Set`: una collezione senza duplicati con metodi pronti per
unione, intersezione e differenza. GDScript **non ha un tipo Set nativo**. Per
ottenere lo stesso comportamento si usa un `Dictionary`, sfruttando il fatto che le
sue chiavi sono per natura sempre uniche: il valore associato ad ogni chiave non
conta nulla, viene usato solo per riempire la coppia (di solito si mette `true`).

---

## Creare un insieme con un Dictionary

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var numeri = {}
    for n in [1, 2, 2, 3, 3, 3]:
        numeri[n] = true

    print(numeri.keys())    # [1, 2, 3] — i duplicati sono spariti
    print(numeri.size())    # 3
{% endhighlight %}

Ogni volta che si assegna `numeri[n] = true` con una chiave già esistente, il
dizionario si limita a riscrivere lo stesso valore: la chiave non si duplica.

---

## Aggiungere, rimuovere, verificare l'appartenenza

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var frutti = {}
    for f in ["mela", "pera", "banana"]:
        frutti[f] = true

    frutti["kiwi"] = true
    print(frutti.has("kiwi"))    # true

    frutti.erase("pera")
    print(frutti.has("pera"))    # false

    # aggiungere un elemento già presente non cambia nulla
    frutti["mela"] = true
    print(frutti.size())          # 4 (invariato)
{% endhighlight %}

---

## Operazioni tra insiemi

Senza i metodi pronti di Ruby (`.union`, `.intersection`, `.difference`), le
operazioni tra insiemi si scrivono con un ciclo `for` e l'operatore `in`.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var classe_a = {"Alice": true, "Bruno": true, "Carla": true, "Davide": true}
    var classe_b = {"Carla": true, "Davide": true, "Elena": true, "Franco": true}

    # unione: tutti gli elementi di entrambi
    var unione = classe_a.duplicate()
    unione.merge(classe_b)
    print("Unione: %s" % [unione.keys()])

    # intersezione: solo gli elementi comuni
    var intersezione = []
    for nome in classe_a:
        if nome in classe_b:
            intersezione.append(nome)
    print("Intersezione: %s" % [intersezione])

    # differenza: elementi di classe_a non in classe_b
    var differenza = []
    for nome in classe_a:
        if not (nome in classe_b):
            differenza.append(nome)
    print("Solo in classe A: %s" % [differenza])
{% endhighlight %}

---

## Rimuovere i duplicati da un array

Una delle applicazioni più comuni per un insieme è **eliminare i duplicati** da un
array esistente.

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var con_doppioni = [1, 2, 2, 3, 3, 3, 4]

    var visti = {}
    var unici = []
    for n in con_doppioni:
        if not visti.has(n):
            visti[n] = true
            unici.append(n)

    print(unici)    # [1, 2, 3, 4]
{% endhighlight %}

---

## Riepilogo: da Ruby a GDScript

| Ruby (`Set`)                    | GDScript equivalente (con `Dictionary`)        |
|----------------------------------|---------------------------------------------------|
| `Set.new`                        | `{}`                                               |
| `s.add(x)` / `s << x`            | `d[x] = true`                                      |
| `s.delete(x)`                    | `d.erase(x)`                                       |
| `s.include?(x)`                  | `d.has(x)` o `x in d`                              |
| `s.length`                       | `d.size()`                                         |
| `s.to_a`                         | `d.keys()`                                         |
| `a \| b` (unione)                | `a.duplicate(); a.merge(b)`                         |
| `a & b` (intersezione)           | ciclo `for` con `in`                               |
| `a - b` (differenza)             | ciclo `for` con `not ... in`                       |

---

## Esercizi

#### Esercizio 5
Scrivi uno script che, dato un array con possibili duplicati, restituisca un array
con soli elementi unici, usando un `Dictionary` di appoggio.

#### Esercizio 6
Scrivi uno script che, dati i nomi degli studenti di due classi (due array), stampi:
quanti sono in totale senza ripetizioni, quanti frequentano entrambe le classi, e i
nomi di chi è solo nella prima classe.

#### Esercizio 7
Scrivi uno script che verifichi se due insiemi (rappresentati come `Dictionary`) sono
disgiunti, cioè non hanno nessun elemento in comune.

#### Esercizio 8
Data una frase, scrivi uno script che trovi e stampi le lettere distinte usate
(senza spazi e senza ripetizioni), usando un `Dictionary` come insieme di appoggio.
