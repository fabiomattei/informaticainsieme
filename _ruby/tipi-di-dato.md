---
title: 'Ruby: tipi di dato'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Tipi di dato

Un'informazione conservata in una variabile ha sempre un **tipo** associato.
Il tipo determina l'insieme di valori che la variabile può assumere e le
operazioni che è possibile eseguire su di essa.

In Ruby il tipo non viene dichiarato esplicitamente: viene assegnato
automaticamente in base al valore che si assegna alla variabile.
Per conoscere il tipo di una variabile si usa il metodo `.class`.

{% highlight ruby %}
puts 42.class        # Integer
puts 3.14.class      # Float
puts true.class      # TrueClass
puts "ciao".class    # String
puts :nome.class     # Symbol
puts nil.class       # NilClass
puts [1,2,3].class   # Array
puts({a: 1}.class)   # Hash
{% endhighlight %}

---

## Numeri interi (Integer)

I numeri interi vengono scritti senza punto decimale.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a = 5
b = 3
puts a + b   # 8
puts a - b   # 2
puts a * b   # 15
puts a / b   # 1  (divisione intera)
puts a % b   # 2  (resto della divisione)
puts a ** b  # 125 (potenza)
{% endhighlight %}

In Ruby la divisione tra due interi restituisce sempre un intero (la parte
decimale viene scartata). Per ottenere un risultato decimale almeno uno dei
due operandi deve essere un `Float`.

Le operazioni possibili sui numeri interi sono:

| Operatore | Significato        | Esempio       |
|-----------|--------------------|---------------|
| `+`       | somma              | `5 + 3` → 8   |
| `-`       | sottrazione        | `5 - 3` → 2   |
| `*`       | prodotto           | `5 * 3` → 15  |
| `/`       | quoziente intero   | `5 / 3` → 1   |
| `%`       | resto (modulo)     | `5 % 3` → 2   |
| `**`      | potenza            | `5 ** 3` → 125|

---

## Numeri decimali (Float)

I numeri a virgola mobile vengono scritti con il punto decimale.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a = 5.0
b = 3.0
puts a / b       # 1.6666666666666667
puts a ** 0.5    # 2.23606797749979 (radice quadrata)
puts 10 / 3.0    # 3.3333333333333335
{% endhighlight %}

---

## Valori booleani (true e false)

Un valore booleano può essere solo `true` (vero) o `false` (falso).
In Ruby le parole chiave sono scritte in minuscolo.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
maggiorenne = true
in_lista = false

puts maggiorenne         # true
puts !maggiorenne        # false (negazione)
puts maggiorenne && true # true  (AND logico)
puts maggiorenne || false # true (OR logico)
{% endhighlight %}

---

## Il valore nil

`nil` rappresenta l'assenza di valore. È il corrispettivo Ruby di `None`
in Python o `null` in altri linguaggi.

{% highlight ruby %}
risultato = nil
puts risultato.nil?   # true
puts risultato.class  # NilClass
{% endhighlight %}

---

## Stringhe (String)

Una stringa è una **sequenza ordinata di caratteri**. In Ruby le stringhe
si delimitano con apici singoli `'...'` o doppi `"..."`.

La differenza principale è che le stringhe con doppi apici supportano
**l'interpolazione**: il contenuto di una variabile può essere inserito
direttamente dentro la stringa con `#{}`.

{% highlight ruby %}
nome = "Giacomo"
cognome = "Leopardi"

puts "Ciao #{nome} #{cognome}!"  # Ciao Giacomo Leopardi!
puts 'Ciao #{nome}'              # Ciao #{nome}  (nessuna interpolazione)
{% endhighlight %}

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
nome = "Giacomo"
cognome = "Leopardi"

nome_cognome = nome + " " + cognome  # concatenazione
ripetuto = nome * 3                  # ripetizione
lunghezza = nome.length              # lunghezza
iniziale = nome[0]                   # primo carattere

puts nome_cognome  # Giacomo Leopardi
puts ripetuto      # GiacomoGiacomoGiacomo
puts lunghezza     # 7
puts iniziale      # G
{% endhighlight %}

Le operazioni principali sulle stringhe sono:

| Operazione       | Significato                         | Esempio                        |
|------------------|-------------------------------------|--------------------------------|
| `a + b`          | concatenazione                      | `"ciao" + "!"` → `"ciao!"`    |
| `a * n`          | ripetizione                         | `"ab" * 3` → `"ababab"`       |
| `a.length`       | numero di caratteri                 | `"ciao".length` → 4           |
| `a[i]`           | carattere in posizione i            | `"ciao"[0]` → `"c"`           |
| `a[i..j]`        | sottostringa dall'indice i a j      | `"ciao"[1..2]` → `"ia"`       |
| `a.upcase`       | tutto maiuscolo                     | `"ciao".upcase` → `"CIAO"`    |
| `a.downcase`     | tutto minuscolo                     | `"CIAO".downcase` → `"ciao"`  |
| `a.reverse`      | stringa capovolta                   | `"ciao".reverse` → `"oaic"`   |
| `a.include?(b)`  | verifica se b è contenuta in a      | `"ciao".include?("ia")` → `true` |

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a = " ciao "
b = " mondo "
puts a + b            # " ciao  mondo "
puts (a + b).length   # 12
puts a.upcase         # " CIAO "
puts a.reverse        # " oaic "
{% endhighlight %}

### Conversione di tipo

Ruby non converte automaticamente i tipi. Per trasformare un valore da un
tipo all'altro si usano i metodi di conversione:

| Metodo    | Converte a | Esempio                      |
|-----------|------------|------------------------------|
| `.to_i`   | Integer    | `"42".to_i` → 42             |
| `.to_f`   | Float      | `"3.14".to_f` → 3.14         |
| `.to_s`   | String     | `42.to_s` → `"42"`           |

---

## Simboli (Symbol)

Un simbolo è simile a una stringa ma è **immutabile** e ha un'identità unica
in memoria. Si scrive con il prefisso `:`.
I simboli vengono usati principalmente come chiavi degli hash e come
identificatori interni al programma.

{% highlight ruby %}
puts :nome.class    # Symbol
puts :nome == :nome # true
puts :nome.to_s     # "nome"
puts "nome".to_sym  # :nome
{% endhighlight %}

---

## Array

Un array è una **sequenza ordinata di elementi** di qualsiasi tipo.
Si definisce con parentesi quadre `[]`. Gli elementi si accedono con
un indice che parte da 0.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
numeri = [10, 20, 30, 40, 50]

puts numeri[0]        # 10  (primo elemento)
puts numeri[-1]       # 50  (ultimo elemento)
puts numeri.length    # 5
puts numeri[1..3].inspect  # [20, 30, 40]

numeri << 60          # aggiunge 60 in coda
puts numeri.length    # 6
{% endhighlight %}

---

## Hash

Un hash è una struttura che associa **chiavi a valori**. Si definisce con
le parentesi graffe `{}`. Le chiavi sono tipicamente simboli.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
persona = { nome: "Mario", eta: 25, citta: "Roma" }

puts persona[:nome]   # Mario
puts persona[:eta]    # 25

persona[:email] = "mario@esempio.it"  # aggiunge una chiave
puts persona.keys.inspect             # [:nome, :eta, :citta, :email]
{% endhighlight %}

I dati in un hash possono essere inseriti in tre modi equivalenti:

{% highlight ruby %}
h1 = { :chiave => "valore" }   # rocket syntax
h2 = { "chiave": "valore" }   # stringa con due punti
h3 = { chiave: "valore" }     # simbolo con due punti (più comune)
{% endhighlight %}

---

## Costanti

Le costanti si riconoscono perché iniziano con la lettera maiuscola.
Ruby mostra un avvertimento se si tenta di modificarne il valore.

{% highlight ruby %}
PI_GRECO = 3.14159
GRAVITA  = 9.81

puts PI_GRECO
puts GRAVITA
{% endhighlight %}

---

## Esercizi

#### Esercizio 8
Scrivi un programma che calcoli area e perimetro di un rettangolo con
lato `a = 4` e lato `b = 7`.

#### Esercizio 9
Scrivi un programma che stampi 10 volte la stringa `"ciao"` usando l'operatore `*`.

#### Esercizio 10
Scrivi un programma che stampi al contrario una stringa letta in input.

#### Esercizio 11
Supponendo di correre 10 km in 42 minuti e 42 secondi, calcola e stampa:
- la velocità media in km/minuto
- la velocità media in km/h
- la velocità media in miglia/h (1 miglio = 1,61 km)

#### Esercizio 12
Scrivi un programma che legga il nome e il cognome dell'utente separatamente
e stampi la frase `"Benvenuto/a, Nome Cognome!"` usando l'interpolazione.

#### Esercizio 13
Scrivi un programma che legga una stringa e stampi il numero di caratteri
che contiene, la stringa in maiuscolo e la stringa capovolta.

#### Esercizio 14
Scrivi un programma che crei un hash con i dati di uno studente (nome, cognome,
classe, voto medio) e stampi ogni coppia chiave-valore su una riga separata.
