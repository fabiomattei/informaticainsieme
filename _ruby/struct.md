---
title: 'Ruby: Struct'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Un oggetto leggero con campi nominati

**Struct** è una scorciatoia per creare classi semplici con attributi
predefiniti senza scrivere `initialize`, getter e setter a mano.
È utile quando si ha bisogno di raggruppare più informazioni correlate
in un unico oggetto, ma non si vuole definire una classe completa.

La differenza tra uno Struct e un hash è che lo Struct ha **campi con
nomi fissi** e si accede ai valori tramite metodi, non tramite chiavi.
La differenza rispetto a una classe è che lo Struct genera automaticamente
il costruttore e i metodi di lettura/scrittura.

---

## Creare uno Struct

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
Punto = Struct.new(:x, :y)

p1 = Punto.new(3, 4)
puts p1.x     # 3
puts p1.y     # 4
puts p1       # #<struct Punto x=3, y=4>
{% endhighlight %}

`Struct.new(:x, :y)` crea una nuova classe con due campi. Si assegna a
una costante con lettera maiuscola (come le classi). Il costruttore
accetta i valori in ordine posizionale.

---

## Leggere e modificare i campi

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
Persona = Struct.new(:nome, :cognome, :eta)

mario = Persona.new("Mario", "Rossi", 25)
puts mario.nome       # Mario
puts mario.cognome    # Rossi
puts mario.eta        # 25

mario.eta = 26        # i campi sono modificabili
puts mario.eta        # 26

puts mario.to_h.inspect
# {:nome=>"Mario", :cognome=>"Rossi", :eta=>26}
{% endhighlight %}

Il metodo `.to_h` converte lo Struct in un hash, utile per
serializzare o confrontare i dati.

---

## Aggiungere metodi a uno Struct

Passando un blocco a `Struct.new` si possono definire metodi
personalizzati, esattamente come in una classe normale.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
Punto = Struct.new(:x, :y) do
  def distanza_dall_origine
    Math.sqrt(x**2 + y**2)
  end

  def to_s
    "(#{x}, #{y})"
  end
end

p1 = Punto.new(3, 4)
puts p1                         # (3, 4)
puts p1.distanza_dall_origine   # 5.0

p2 = Punto.new(0, 0)
puts p2.distanza_dall_origine   # 0.0
{% endhighlight %}

---

## Struct in array

Una delle applicazioni più comuni è creare array di struct per
rappresentare tabelle di dati strutturati.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
Studente = Struct.new(:nome, :voto)

classe = [
  Studente.new("Alice", 9),
  Studente.new("Bruno", 6),
  Studente.new("Carla", 8),
  Studente.new("Davide", 7)
]

for studente in classe
  puts "#{studente.nome}: #{studente.voto}"
end

promossi = classe.select { |s| s.voto >= 6 }
puts "Promossi: #{promossi.map(&:nome).inspect}"

migliore = classe.max_by { |s| s.voto }
puts "Migliore: #{migliore.nome} con #{migliore.voto}"
{% endhighlight %}

---

## Confrontare gli Struct

Due istanze dello stesso Struct sono uguali se tutti i loro campi hanno
lo stesso valore.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
Punto = Struct.new(:x, :y)

a = Punto.new(1, 2)
b = Punto.new(1, 2)
c = Punto.new(3, 4)

puts a == b    # true
puts a == c    # false
{% endhighlight %}

Questo comportamento è automatico: con le classi normali `==` confronta
l'identità degli oggetti (sono lo stesso oggetto in memoria?), non i
loro valori.

---

## Struct vs Hash vs Classe

| Caratteristica         | Struct       | Hash         | Classe       |
|------------------------|:------------:|:------------:|:------------:|
| Campi nominati fissi   | sì           | no           | sì           |
| Costruttore automatico | sì           | —            | no           |
| Getter/setter automatici | sì         | —            | no           |
| Metodi personalizzati  | sì           | no           | sì           |
| Confronto per valore   | sì           | sì           | no (default) |
| Flessibilità dei campi | bassa        | alta         | media        |

Usare uno **Struct** quando:
- il numero di campi è fisso e noto in anticipo
- si vuole evitare la verbosità di una classe completa
- si ha bisogno del confronto per valore

---

## Esercizi

#### Esercizio 6
Definisci uno Struct `Rettangolo` con i campi `base` e `altezza`.
Aggiungi i metodi `area` e `perimetro`. Crea tre istanze e stampa
area e perimetro di ciascuna.

#### Esercizio 7
Definisci uno Struct `Prodotto` con i campi `nome`, `prezzo` e
`quantita`. Crea un array di almeno 5 prodotti, quindi stampa:
il prodotto più costoso, la somma del valore totale in magazzino
(`prezzo * quantita` per ciascuno), e tutti i prodotti con prezzo
inferiore a 10 €.

#### Esercizio 8
Definisci uno Struct `Studente` con `nome`, `cognome` e `voto_medio`.
Crea un array di almeno 6 studenti, ordinali per voto medio decrescente
e stampa la classifica con la posizione.

#### Esercizio 9
Definisci uno Struct `Contatto` con `nome`, `telefono` e `email`.
Simula una rubrica: crea 4 contatti, inseriscili in un array e scrivi
un programma che letto un nome in input stampi i dati del contatto
corrispondente (o "Non trovato" se il nome non esiste).
