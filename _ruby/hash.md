---
title: 'Ruby: gli hash'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Chiavi e valori

Se gli array sono cassettiere con i cassetti numerati, gli **hash** sono
cassettiere con i cassetti **etichettati**. Un hash associa ogni
**valore** a una **chiave** univoca che lo identifica. Si usa questa
struttura quando non è comodo lavorare con indici numerici ma si vuole
recuperare le informazioni tramite un nome descrittivo.

Gli hash sono un tipo **mutabile** che contiene elementi formati da una
coppia chiave/valore. Le chiavi sono tipicamente simboli (`:nome`) ma
possono essere stringhe, numeri o qualsiasi oggetto immutabile.

---

## Creare un hash

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
vuoto   = {}
persona = { nome: "Mario", cognome: "Rossi", eta: 25 }

puts persona[:nome]     # Mario
puts persona[:cognome]  # Rossi
puts persona[:eta]      # 25
puts persona.length     # 3
{% endhighlight %}

La sintassi `nome: "Mario"` è la forma più comune in Ruby moderno.
È equivalente a scrivere `:nome => "Mario"` (rocket syntax).

Ruby accetta tre forme di scrittura, tutte equivalenti:

{% highlight ruby %}
h1 = { :nome => "Mario" }    # rocket syntax (chiave simbolo)
h2 = { "nome" => "Mario" }   # rocket syntax (chiave stringa)
h3 = { nome: "Mario" }       # sintassi moderna (chiave simbolo)
{% endhighlight %}

---

## Accedere agli elementi

Si usa la notazione `hash[chiave]`. Se la chiave non esiste, Ruby
restituisce `nil` invece di generare un errore.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
voti = { fisica: 8, matematica: 6, storia: 7, inglese: 9 }

puts voti[:fisica]      # 8
puts voti[:storia]      # 7
puts voti[:arte]        # nil  (chiave inesistente)

# fetch con valore di default
puts voti.fetch(:arte, 0)   # 0  (default se la chiave manca)
{% endhighlight %}

---

## Aggiungere, modificare e rimuovere

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
persona = { nome: "Mario", eta: 25 }

# aggiungere una nuova chiave
persona[:citta] = "Roma"
puts persona.inspect    # {:nome=>"Mario", :eta=>25, :citta=>"Roma"}

# modificare un valore esistente
persona[:eta] = 26
puts persona[:eta]      # 26

# rimuovere una chiave
persona.delete(:citta)
puts persona.inspect    # {:nome=>"Mario", :eta=>26}
{% endhighlight %}

---

## Verificare la presenza di una chiave o di un valore

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
voti = { fisica: 8, matematica: 6, storia: 7 }

if voti.key?(:fisica)
  puts "La fisica è presente"
end

unless voti.key?(:arte)
  puts "L'arte non è presente"
end

if voti.value?(6)
  puts "C'è almeno un voto pari a 6"
end
{% endhighlight %}

---

## Visitare un hash

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
stati_e_capitali = {
  italia:    "Roma",
  francia:   "Parigi",
  germania:  "Berlino",
  spagna:    "Madrid"
}

for stato, capitale in stati_e_capitali
  puts "#{stato}: #{capitale}"
end
{% endhighlight %}

Lo stile idiomatico Ruby usa `each`:

{% highlight ruby %}
stati_e_capitali.each do |stato, capitale|
  puts "La capitale di #{stato} è #{capitale}"
end
{% endhighlight %}

Si possono anche visitare solo le chiavi o solo i valori:

{% highlight ruby %}
puts voti.keys.inspect    # [:fisica, :matematica, :storia]
puts voti.values.inspect  # [8, 6, 7]
{% endhighlight %}

---

## Metodi utili

| Metodo              | Significato                                        |
|---------------------|----------------------------------------------------|
| `.length`           | numero di coppie chiave/valore                     |
| `.keys`             | array di tutte le chiavi                           |
| `.values`           | array di tutti i valori                            |
| `.key?(k)`          | `true` se la chiave `k` esiste                     |
| `.value?(v)`        | `true` se il valore `v` esiste                     |
| `.delete(k)`        | rimuove la coppia con chiave `k`                   |
| `.merge(altro)`     | unisce due hash (i valori del secondo prevalgono)  |
| `.select { }`       | restituisce un nuovo hash con le coppie filtrate   |
| `.any? { }`         | `true` se almeno una coppia soddisfa la condizione |
| `.min_by { }`       | coppia con il valore minimo secondo il blocco      |
| `.max_by { }`       | coppia con il valore massimo secondo il blocco     |

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
voti = { fisica: 8, matematica: 6, storia: 7, inglese: 9 }

# materia con il voto più basso
minimo = voti.min_by { |materia, voto| voto }
puts "Voto più basso: #{minimo[0]} con #{minimo[1]}"

# seleziona solo i voti sufficienti (>= 6)
sufficienti = voti.select { |materia, voto| voto >= 6 }
puts sufficienti.inspect

# unisce due hash
extra = { educazione_fisica: 9, arte: 7 }
completo = voti.merge(extra)
puts completo.length    # 6
{% endhighlight %}

---

## Esercizi

#### Esercizio 7
Definisci un hash `persona` con le chiavi `nome`, `cognome` e `indirizzo`.
Scrivi un ciclo che stampi tutte le coppie chiave/valore.

#### Esercizio 8
Scrivi un programma che letto un numero N generi un hash in cui le chiavi
sono i numeri da 1 a N e i valori sono i loro quadrati.
Esempio (N=5): `{1=>1, 2=>4, 3=>9, 4=>16, 5=>25}`

#### Esercizio 9
Scrivi un programma che verifichi se due hash hanno le stesse chiavi.

#### Esercizio 10
Dati due hash con valori numerici, scrivi un programma che ne crei un
terzo contenente tutte le coppie di entrambi (unione).

#### Esercizio 11
Dato un hash `voti` (materia → voto), scrivi un programma che stampi
il nome della materia con il voto più alto e quello con il voto più basso.

#### Esercizio 12
Crea un array di hash, ciascuno che descrive uno studente con `nome`,
`cognome` e `voto_medio`. Scrivi un ciclo che stampi solo gli studenti
con voto medio maggiore o uguale a 7.
