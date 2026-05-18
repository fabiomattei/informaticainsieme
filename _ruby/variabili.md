---
title: 'Ruby: variabili'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Contenitori di informazioni

Le variabili sono **contenitori di informazioni**. Possiamo pensare a una
variabile come a un cassetto dotato di etichetta: ogni variabile ha un
**nome** che serve per ritrovarla facilmente, e un **valore** che è
l'informazione conservata al suo interno.

Per creare una variabile in Ruby si usa l'operatore di assegnazione `=`:

{% highlight ruby %}
x = 5
y = "Mario"
z = 3.14
{% endhighlight %}

Questa non è un'equazione matematica ma un'**assegnazione**. L'istruzione
`x = 5` si legge: *assegno alla variabile x il numero 5*. Significa che
nella memoria del computer viene riservato uno spazio con l'etichetta `x`,
e all'interno viene posto il valore `5`.

- il simbolo a **sinistra** del `=` è il **nome della variabile**
- il simbolo a **destra** del `=` è il **valore da conservare**

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
numero_panini = 6
costo_panino  = 2
costo_totale  = numero_panini * costo_panino
puts costo_totale
{% endhighlight %}

Osserva che:
- alle variabili sono stati dati nomi significativi
- nelle variabili si memorizzano valori ma non le unità di misura
- il simbolo `*` indica la moltiplicazione

---

## Nomi di variabile

In Ruby il nome di una variabile locale segue queste regole:

- deve iniziare con una **lettera minuscola** o con il trattino basso `_`
- non può iniziare con un numero
- può contenere lettere, numeri e trattino basso (`a-z`, `0-9`, `_`)
- i nomi sono **case-sensitive**: `volume`, `Volume` e `VOLUME` sono tre
  variabili distinte

La convenzione Ruby è lo **snake_case**: parole separate da trattino basso,
tutte in minuscolo. Esempi di nomi corretti:

{% highlight ruby %}
nome = "Alice"
cognome = "Rossi"
eta = 17
voto_medio = 8.5
numero_di_studenti = 25
{% endhighlight %}

---

## Leggere e scrivere variabili

`puts` scrive il valore di una variabile sulla console. `gets.chomp` legge
una riga di testo inserita dall'utente (senza il carattere di fine riga).

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Come ti chiami? "
nome = gets.chomp
puts "Ciao, #{nome}!"
{% endhighlight %}

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire. Osserva come il
valore di una variabile può essere aggiornato nel corso del programma.

{% highlight ruby %}
contatore = 0
puts contatore    # 0
contatore = contatore + 1
puts contatore    # 1
contatore = contatore + 1
puts contatore    # 2
{% endhighlight %}

La riga `contatore = contatore + 1` si legge: *prendi il valore attuale
di `contatore`, aggiungi 1, e rimetti il risultato in `contatore`*.

---

## Operatori di assegnazione composti

Ruby offre operatori abbreviati per le operazioni più comuni sulla stessa
variabile:

| Operatore | Equivalente a       | Esempio                |
|-----------|---------------------|------------------------|
| `+=`      | `a = a + n`         | `x += 3`               |
| `-=`      | `a = a - n`         | `x -= 3`               |
| `*=`      | `a = a * n`         | `x *= 2`               |
| `/=`      | `a = a / n`         | `x /= 2`               |
| `**=`     | `a = a ** n`        | `x **= 2`              |
| `%=`      | `a = a % n`         | `x %= 3`               |

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
punteggio = 100
punteggio += 50    # 150
punteggio -= 20    # 130
punteggio *= 2     # 260
puts punteggio
{% endhighlight %}

---

## Assegnazione multipla (parallel assignment)

Ruby permette di assegnare più variabili in una sola istruzione.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a, b, c = 10, 20, 30
puts a    # 10
puts b    # 20
puts c    # 30
{% endhighlight %}

Questo si chiama **assegnazione parallela**: Ruby valuta tutti i valori
a destra prima di assegnarli alle variabili a sinistra.

---

## Scambiare due variabili

In molti linguaggi scambiare il valore di due variabili richiede una
variabile temporanea. In Ruby l'assegnazione parallela permette di farlo
in una sola riga.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a = "primo"
b = "secondo"

puts "Prima: a=#{a}, b=#{b}"
a, b = b, a
puts "Dopo:  a=#{a}, b=#{b}"
{% endhighlight %}

---

## Il tipo di una variabile

In Ruby non è necessario dichiarare il tipo: viene determinato
automaticamente dal valore assegnato. Il metodo `.class` restituisce
il tipo corrente.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
x = 42
puts x.class      # Integer

x = 3.14
puts x.class      # Float

x = "ciao"
puts x.class      # String

x = true
puts x.class      # TrueClass
{% endhighlight %}

La stessa variabile può contenere valori di tipo diverso in momenti
diversi: in Ruby il tipo è legato al **valore**, non alla variabile.

---

## Tabella di tracciamento

Quando un programma modifica le variabili in sequenza, è utile disegnare
una **tabella di tracciamento** per seguire i cambiamenti.

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire, poi compila la
tabella di tracciamento.

{% highlight ruby %}
a = 3
b = 5
c = a + b
a = c * 2
b = a - c
puts a
puts b
puts c
{% endhighlight %}

| Istruzione   | a  | b  | c  |
|--------------|----|----|-----|
| `a = 3`      | 3  | —  | —  |
| `b = 5`      | 3  | 5  | —  |
| `c = a + b`  | 3  | 5  | 8  |
| `a = c * 2`  | 16 | 5  | 8  |
| `b = a - c`  | 16 | 8  | 8  |

---

## Esercizi

#### Esercizio 9
Scrivi un programma che memorizzi in tre variabili i lati di un triangolo
rettangolo (base, altezza e ipotenusa) e calcoli e stampi il perimetro.

#### Esercizio 10
Un'automobile percorre 120 km con 8 litri di carburante. Scrivi un
programma che calcoli e stampi i km percorsi per litro e quanti litri
servono per percorrere 500 km.

#### Esercizio 11
Scrivi un programma che legga il nome, il cognome e l'anno di nascita
dell'utente e stampi una frase del tipo:
`"Mario Rossi ha 25 anni."` (calcola l'età dall'anno corrente 2026).

#### Esercizio 12
Scrivi un programma che legga la base e l'altezza di un rettangolo e
calcoli area e perimetro.

#### Esercizio 13
Scrivi un programma che legga il prezzo di un articolo e la percentuale
di sconto, e calcoli il prezzo finale scontato.

#### Esercizio 14
Scrivi un programma che legga tre numeri, li memorizzi in tre variabili
e stampi la loro somma, il loro prodotto e la loro media.

#### Esercizio 15
Scrivi un programma che legga due numeri interi e scambi il loro valore
usando l'assegnazione parallela, stampando entrambi prima e dopo lo scambio.
