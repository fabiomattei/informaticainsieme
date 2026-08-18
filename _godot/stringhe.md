---
title: 'Godot: le stringhe di testo'
date: '2026-08-18T09:15:00+01:00'
author: Fabio Mattei
layout: page
---

## Una sequenza di caratteri

Una stringa di testo è una **sequenza ordinata di caratteri**, esattamente come visto
per Ruby: il computer memorizza ogni carattere come un numero secondo una tabella
(ASCII/Unicode), e "CIAO" è in realtà una sequenza di quattro numeri.

| C  | I  | A  | O  |
|----|----|----|-----|
| 67 | 73 | 65 | 79 |

---

## Inizializzare una stringa

In GDScript le stringhe si delimitano con **apici singoli** `'...'` o **doppi**
`"..."`, senza alcuna differenza di comportamento (a differenza di Ruby, dove i doppi
apici attivano l'interpolazione). Il valore da inserire dinamicamente in una stringa
si costruisce con l'**operatore %**, visto nella pagina sull'[output]({{ site.baseurl }}{% link _godot/output.md %}.html).

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var nome = "Godot"
    print("Benvenuto in %s!" % nome)     # Benvenuto in Godot!
    print("2 + 2 = %d" % (2 + 2))         # 2 + 2 = 4
{% endhighlight %}

---

## Lunghezza di una stringa

Il metodo `.length()` restituisce il numero di caratteri contenuti nella stringa.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var saluto = "ciao"
    print(saluto.length())     # 4

    var parola = "informatica"
    print(parola.length())     # 11

    print("".length())          # 0  (stringa vuota)
{% endhighlight %}

---

## Accedere a un singolo carattere

GDScript numera ogni carattere con un **indice** che parte da 0. Il carattere
all'indice `i` si ottiene con `stringa[i]`. Gli indici negativi contano dalla fine:
`-1` è l'ultimo carattere, `-2` il penultimo.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var saluto = "ciao"
    print(saluto[0])    # c
    print(saluto[1])    # i
    print(saluto[3])    # o
    print(saluto[-1])   # o  (ultimo carattere)
    print(saluto[-2])   # a  (penultimo)
{% endhighlight %}

| lettera          | c  | i  | a  | o  |
|-------------------|----|----|----|----|
| indice            | 0  | 1  | 2  | 3  |
| indice negativo   | -4 | -3 | -2 | -1 |

---

## Sottostringhe

Con `.substr(inizio, lunghezza)` si estrae una porzione di stringa a partire da un
indice, per una data lunghezza. A differenza del `range` di Ruby (`inizio..fine`),
GDScript vuole **quanti caratteri prendere**, non l'indice finale.

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var parola = "informatica"
    print(parola.substr(0, 5))    # infor
    print(parola.substr(5, 6))    # matica
    print(parola.substr(0, 4))    # info
    print(parola.substr(parola.length() - 5, 5))   # atica (ultimi 5 caratteri)
{% endhighlight %}

---

## Confrontare le stringhe

Le stringhe si confrontano con gli stessi operatori dei numeri. Il confronto avviene
carattere per carattere seguendo l'ordine Unicode: le lettere maiuscole precedono le
minuscole.

| Operatore | Significato                    |
|-----------|----------------------------------|
| `==`      | uguale                           |
| `!=`      | diverso                          |
| `<`       | viene prima (alfabeticamente)    |
| `>`       | viene dopo                       |
| `<=`      | prima o uguale                   |
| `>=`      | dopo o uguale                    |

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    print("ciao" == "ciao")       # true
    print("mattino" > "sera")     # false  (m viene prima di s)
    print("mattino" > "mattina")  # true   (o viene dopo a)
    print("A" < "a")              # true   (maiuscole prima di minuscole)

    var a = "banana"
    var b = "arancia"
    if a < b:
        print(a)
    else:
        print(b)
{% endhighlight %}

---

## Metodi sulle stringhe

GDScript offre molti metodi per trasformare e interrogare le stringhe.

#### Esercizio 6
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var s = "Ciao Mondo"
    print(s.to_upper())          # CIAO MONDO
    print(s.to_lower())          # ciao mondo
    print(s.reverse())           # odnoM oaiC
    print(s.length())            # 10
    print(s.find("Mondo") != -1) # true
    print(s.find("mondo") != -1) # false  (maiuscole contano)
{% endhighlight %}

#### Esercizio 7
`.replace()` sostituisce tutte le occorrenze di un testo con un altro.

{% highlight gdscript %}
func _ready():
    var frase = "il gatto sul tetto"
    print(frase.replace("t", "T"))         # il gaTTo sul TeTTo
    print(frase.replace("tetto", "prato")) # il gatto sul prato
{% endhighlight %}

#### Esercizio 8
`.split()` divide una stringa in un array di sottostringhe.

{% highlight gdscript %}
func _ready():
    var frase = "uno due tre quattro"
    var parole = frase.split(" ")
    print(parole.size())    # 4
    for parola in parole:
        print(parola)
{% endhighlight %}

Tabella riepilogativa dei metodi principali:

| Metodo             | Significato                                       |
|---------------------|-----------------------------------------------------|
| `.length()`         | numero di caratteri                                 |
| `.to_upper()`       | tutto maiuscolo                                     |
| `.to_lower()`       | tutto minuscolo                                     |
| `.reverse()`        | stringa capovolta                                   |
| `.find(sub) != -1`  | `true` se `sub` è contenuta nella stringa           |
| `.replace(a, b)`    | sostituisce tutte le occorrenze di `a` con `b`      |
| `.split(sep)`       | divide in array usando `sep` come separatore        |
| `.strip_edges()`    | rimuove spazi iniziali e finali                     |
| `.begins_with(s)`   | `true` se inizia con `s`                            |
| `.ends_with(s)`     | `true` se termina con `s`                           |

---

## Visitare una stringa

Per esaminare ogni carattere di una stringa si usa un ciclo. GDScript, come Ruby,
permette di iterare direttamente sui caratteri con un `for`, senza bisogno di un
indice esplicito.

#### Esercizio 9
Copia il seguente codice dentro `_ready()` e fallo eseguire. Il programma classifica
ogni carattere come vocale o consonante.

{% highlight gdscript %}
func _ready():
    var parola = "ciao"
    var vocali = "aeiouAEIOU"
    for lettera in parola:
        if vocali.find(lettera) != -1:
            print("%s → vocale" % lettera)
        else:
            print("%s → consonante" % lettera)
{% endhighlight %}

Tabella di tracciamento per la parola "ciao":

| lettera | output      |
|:-------:|-------------|
| c       | consonante  |
| i       | vocale      |
| a       | vocale      |
| o       | vocale      |

Si può anche visitare una stringa con un ciclo `while` e un indice esplicito, utile
quando serve conoscere la posizione corrente.

{% highlight gdscript %}
func _ready():
    var parola = "ciao"
    var indice = 0
    while indice < parola.length():
        print(parola[indice])
        indice += 1
{% endhighlight %}

---

## Esercizi

#### Esercizio 10
Scrivi uno script che, date due stringhe, le stampi in ordine di lunghezza (prima la
più corta).

#### Esercizio 11
Scrivi uno script che, data una stringa, la stampi tante volte quanti sono i
caratteri che la compongono.

#### Esercizio 12
Scrivi uno script che visiti una stringa e conti quante vocali contiene.

#### Esercizio 13
Scrivi uno script che, data una stringa, crei una nuova stringa sostituendo tutte le
`s` (maiuscole e minuscole) con `$` e tutte le `e` (maiuscole e minuscole) con `€`.

#### Esercizio 14
Scrivi uno script che applichi il **cifrario di Cesare** a una stringa: ogni lettera
viene spostata in avanti di `k` posizioni nell'alfabeto (usa `.unicode_at(i)` per
ottenere il codice di un carattere e `char()` per tornare da un codice a un
carattere).
