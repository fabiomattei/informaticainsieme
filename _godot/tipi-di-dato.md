---
title: 'Godot: tipi di dato'
date: '2026-08-18T09:10:00+01:00'
author: Fabio Mattei
layout: page
---

## Tipi di dato

Un'informazione conservata in una variabile ha sempre un **tipo** associato. Il tipo
determina l'insieme di valori che la variabile può assumere e le operazioni che è
possibile eseguire su di essa.

Come in Ruby, in GDScript il tipo non deve essere dichiarato esplicitamente: viene
assegnato automaticamente in base al valore. Per conoscerlo si usano `typeof()` e
`type_string()`, visti nella pagina sulle [variabili]({{ site.baseurl }}{% link _godot/variabili.md %}.html).

{% highlight gdscript %}
func _ready():
    print(type_string(typeof(42)))          # int
    print(type_string(typeof(3.14)))         # float
    print(type_string(typeof(true)))         # bool
    print(type_string(typeof("ciao")))       # String
    print(type_string(typeof(null)))         # Nil
    print(type_string(typeof([1,2,3])))      # Array
    print(type_string(typeof({"a": 1})))     # Dictionary
{% endhighlight %}

---

## Numeri interi (int)

I numeri interi vengono scritti senza punto decimale.

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var a = 5
    var b = 3
    print(a + b)          # 8
    print(a - b)          # 2
    print(a * b)          # 15
    print(a / b)          # 1  (divisione intera)
    print(a % b)          # 2  (resto della divisione)
    print(pow(a, b))      # 125.0 (potenza, restituisce sempre un float)
{% endhighlight %}

Come in Ruby, la divisione tra due interi restituisce sempre un intero: la parte
decimale viene scartata (troncata verso lo zero). Per ottenere un risultato decimale
almeno uno dei due operandi deve essere un `float`.

| Operatore   | Significato       | Esempio           |
|-------------|--------------------|--------------------|
| `+`         | somma              | `5 + 3` → 8        |
| `-`         | sottrazione        | `5 - 3` → 2        |
| `*`         | prodotto           | `5 * 3` → 15       |
| `/`         | quoziente intero   | `5 / 3` → 1        |
| `%`         | resto (modulo)     | `5 % 3` → 2        |
| `pow(a, b)` | potenza            | `pow(5, 3)` → 125.0|

---

## Numeri decimali (float)

I numeri a virgola mobile vengono scritti con il punto decimale.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var a = 5.0
    var b = 3.0
    print(a / b)               # 1.666667
    print(pow(a, 0.5))         # 2.236068 (radice quadrata)
    print(10 / 3.0)            # 3.333333
    print(sqrt(a))             # 2.236068 (funzione dedicata alla radice quadrata)
{% endhighlight %}

---

## Valori booleani (true e false)

Un valore booleano può essere solo `true` o `false`, scritti in minuscolo come in Ruby.

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var maggiorenne = true
    var in_lista = false

    print(maggiorenne)               # true
    print(!maggiorenne)               # false (negazione)
    print(maggiorenne and true)      # true  (AND logico)
    print(maggiorenne or false)      # true  (OR logico)
{% endhighlight %}

---

## Il valore null

`null` rappresenta l'assenza di valore: è l'equivalente GDScript di `nil` in Ruby o
`None` in Python.

{% highlight gdscript %}
func _ready():
    var risultato = null
    print(risultato == null)                 # true
    print(type_string(typeof(risultato)))    # Nil
{% endhighlight %}

---

## Stringhe (String)

Una stringa è una **sequenza ordinata di caratteri**. In GDScript si delimitano con
apici singoli `'...'` o doppi `"..."`, senza differenze funzionali tra i due (a
differenza di Ruby, dove i doppi apici abilitano l'interpolazione).

{% highlight gdscript %}
func _ready():
    var nome = "Giacomo"
    var cognome = 'Leopardi'
    print("Ciao %s %s!" % [nome, cognome])   # Ciao Giacomo Leopardi!
{% endhighlight %}

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var nome = "Giacomo"
    var cognome = "Leopardi"

    var nome_cognome = nome + " " + cognome    # concatenazione
    var ripetuto = nome.repeat(3)               # ripetizione
    var lunghezza = nome.length()               # lunghezza
    var iniziale = nome[0]                      # primo carattere

    print(nome_cognome)   # Giacomo Leopardi
    print(ripetuto)       # GiacomoGiacomoGiacomo
    print(lunghezza)      # 7
    print(iniziale)       # G
{% endhighlight %}

Le operazioni principali sulle stringhe sono:

| Operazione        | Significato                          | Esempio                          |
|-------------------|---------------------------------------|------------------------------------|
| `a + b`           | concatenazione                        | `"ciao" + "!"` → `"ciao!"`        |
| `a.repeat(n)`     | ripetizione                           | `"ab".repeat(3)` → `"ababab"`     |
| `a.length()`      | numero di caratteri                   | `"ciao".length()` → 4             |
| `a[i]`            | carattere in posizione i              | `"ciao"[0]` → `"c"`               |
| `a.substr(i, n)`  | sottostringa da i, lunga n caratteri  | `"ciao".substr(1, 2)` → `"ia"`    |
| `a.to_upper()`    | tutto maiuscolo                       | `"ciao".to_upper()` → `"CIAO"`    |
| `a.to_lower()`    | tutto minuscolo                       | `"CIAO".to_lower()` → `"ciao"`    |
| `a.reverse()`     | stringa capovolta                     | `"ciao".reverse()` → `"oaic"`     |
| `a.contains(b)`   | verifica se b è contenuta in a        | `"ciao".contains("ia")` → `true`  |

Nota: `a.contains(b)` è disponibile da Godot 4.3; nelle versioni precedenti si usa
`b in a` oppure `a.find(b) != -1`.

### Conversione di tipo

GDScript non converte automaticamente i tipi. Vedremo tutti i dettagli nella pagina
dedicata alle [conversioni di tipo]({{ site.baseurl }}{% link _godot/conversioni-di-tipo.md %}.html);
in anteprima, le funzioni principali sono:

| Funzione   | Converte a | Esempio                 |
|------------|------------|--------------------------|
| `int(x)`   | int        | `int("42")` → 42         |
| `float(x)` | float      | `float("3.14")` → 3.14   |
| `str(x)`   | String     | `str(42)` → `"42"`       |

---

## Array

Un array è una **sequenza ordinata di elementi** di qualsiasi tipo. Si definisce con
parentesi quadre `[]`. Gli elementi si accedono con un indice che parte da 0.

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var numeri = [10, 20, 30, 40, 50]

    print(numeri[0])          # 10  (primo elemento)
    print(numeri[-1])         # 50  (ultimo elemento)
    print(numeri.size())      # 5
    print(numeri.slice(1, 4)) # [20, 30, 40]

    numeri.append(60)         # aggiunge 60 in coda
    print(numeri.size())      # 6
{% endhighlight %}

---

## Dictionary (l'equivalente dell'hash)

Un `Dictionary` associa **chiavi a valori**. Si definisce con le parentesi graffe
`{}`. Le chiavi possono essere stringhe, numeri o quasi ogni altro valore.

#### Esercizio 6
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var persona = {"nome": "Mario", "eta": 25, "citta": "Roma"}

    print(persona["nome"])   # Mario
    print(persona["eta"])    # 25

    persona["email"] = "mario@esempio.it"   # aggiunge una chiave
    print(persona.keys())                    # [nome, eta, citta, email]
{% endhighlight %}

GDScript accetta anche una sintassi alternativa, simile a quella di Ruby con i
simboli, quando le chiavi sono identificatori validi:

{% highlight gdscript %}
var h1 = {"chiave": "valore"}   # sintassi con stringa
var h2 = {chiave = "valore"}    # sintassi "Lua-like", equivalente
print(h2.chiave)                 # accesso col punto, come se fosse un attributo
{% endhighlight %}

Approfondiremo array e dizionari nelle pagine dedicate: [Array]({{ site.baseurl }}{% link _godot/array.md %}.html)
e [Dizionari]({{ site.baseurl }}{% link _godot/dizionari.md %}.html).

---

## Costanti

Le costanti si dichiarano con `const` invece di `var`. A differenza di Ruby, dove una
costante è solo una convenzione di nome (lettera maiuscola) con un semplice
avvertimento in caso di modifica, in GDScript `const` è **imposta dal linguaggio**:
tentare di riassegnarla è un errore rilevato prima ancora di eseguire lo script.

{% highlight gdscript %}
const PI_GRECO = 3.14159
const GRAVITA = 9.81

func _ready():
    print(PI_GRECO)
    print(GRAVITA)
{% endhighlight %}

---

## Esercizi

#### Esercizio 7
Scrivi uno script che calcoli area e perimetro di un rettangolo con lato `a = 4` e
lato `b = 7`.

#### Esercizio 8
Scrivi uno script che stampi 10 volte la stringa `"ciao"` usando `.repeat()`.

#### Esercizio 9
Scrivi uno script che, data una stringa, la stampi al contrario con `.reverse()`.

#### Esercizio 10
Supponendo di correre 10 km in 42 minuti e 42 secondi, calcola e stampa: la velocità
media in km/minuto, la velocità media in km/h, la velocità media in miglia/h
(1 miglio = 1,61 km).

#### Esercizio 11
Scrivi uno script che crei un `Dictionary` con i dati di uno studente (nome, cognome,
classe, voto medio) e stampi ogni coppia chiave-valore su una riga separata,
usando un ciclo `for` (vedi [il ciclo for]({{ site.baseurl }}{% link _godot/il-ciclo-for.md %}.html)).
