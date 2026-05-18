---
title: 'Ruby: classi'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Introduzione

Nella programmazione procedurale dati e funzioni sono entità separate: le
variabili contengono le informazioni, le funzioni contengono gli algoritmi
che le elaborano. Quando il codice cresce questa separazione crea problemi:
variabili che cambiano valore da parti del programma lontane tra loro,
funzioni con nomi in conflitto scritte da persone diverse, comportamenti
inaspettati difficili da rintracciare.

La **programmazione ad oggetti** risolve questi problemi raggruppando insieme
le informazioni e gli algoritmi che le riguardano all'interno di un'unica
struttura chiamata **classe**. Le informazioni si chiamano **attributi**, gli
algoritmi si chiamano **metodi**.

## La prima classe

In Ruby una classe si definisce con la parola chiave `class` e si chiude
con `end`. Il nome inizia sempre con la lettera maiuscola.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
class Cerchio
  @@pigreco = 3.14

  def initialize(raggio)
    @raggio = raggio
  end

  def calcola_area
    @raggio ** 2 * @@pigreco
  end

  def calcola_circonferenza
    2 * @@pigreco * @raggio
  end
end

# main
cerchio = Cerchio.new(5)
puts cerchio.calcola_area
puts cerchio.calcola_circonferenza
{% endhighlight %}

La classe `Cerchio` ha un **attributo di classe** `@@pigreco`: ha lo stesso
valore per tutte le istanze. Ha anche un **attributo di istanza** `@raggio`:
ogni cerchio ha il suo valore personale. Infine ha due **metodi** che operano
sugli attributi per restituire i risultati.

## Attributi di istanza e di classe

Gli attributi si distinguono dal prefisso:

| Prefisso | Tipo             | Condiviso tra istanze? |
|----------|------------------|------------------------|
| `@`      | di istanza       | no — ogni oggetto ha il suo valore |
| `@@`     | di classe        | sì — uguale per tutti gli oggetti della classe |

Il metodo `initialize` è il **costruttore**: viene chiamato automaticamente
da Ruby quando si crea una nuova istanza con `.new`. I parametri che passiamo
a `.new` arrivano direttamente a `initialize`.

{% highlight ruby %}
cerchio_piccolo = Cerchio.new(3)
cerchio_grande  = Cerchio.new(10)
# @raggio è diverso per ciascuno; @@pigreco è lo stesso
{% endhighlight %}

## Classi e istanze

La **classe** è un costrutto astratto: descrive la struttura e il
comportamento di un oggetto senza riferirsi a nessun oggetto specifico.
L'**istanza** è la materializzazione concreta di una classe.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
class Alunno
  def initialize(nome, cognome)
    @nome = nome
    @cognome = cognome
  end

  def saluta
    "Ciao, mi chiamo #{@nome} #{@cognome}"
  end
end

# main
mario = Alunno.new("Mario", "Rossi")
rita  = Alunno.new("Rita", "Morelli")

puts mario.saluta
puts rita.saluta
{% endhighlight %}

`mario` e `rita` sono due istanze distinte della stessa classe `Alunno`.
Condividono la struttura (attributi e metodi) ma ognuna ha i propri valori.

## Accessor: leggere e modificare gli attributi

Gli attributi di istanza sono privati per default: non sono accessibili
direttamente dall'esterno della classe. Per renderli leggibili o modificabili
si usano i metodi **accessor**.

Ruby offre tre scorciatoie:

| Metodo            | Effetto                              |
|-------------------|--------------------------------------|
| `attr_reader`     | genera solo il metodo di lettura     |
| `attr_writer`     | genera solo il metodo di scrittura   |
| `attr_accessor`   | genera entrambi                      |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
class Alunno
  attr_reader   :nome, :cognome
  attr_accessor :voto_medio

  def initialize(nome, cognome)
    @nome = nome
    @cognome = cognome
    @voto_medio = 0
  end

  def saluta
    "Ciao, mi chiamo #{@nome} #{@cognome}"
  end
end

# main
mario = Alunno.new("Mario", "Rossi")
puts mario.nome         # lettura consentita
mario.voto_medio = 8.5  # scrittura consentita
puts mario.voto_medio
# mario.nome = "Luigi"  # errore: nome è solo in lettura
{% endhighlight %}

## Ereditarietà

La **ereditarietà** permette di creare una nuova classe che estende una
classe esistente, ereditandone attributi e metodi. Si usa quando due classi
condividono una parte comune. La classe figlia indica la classe padre con il
simbolo `<`.

La parola chiave `super` richiama il metodo omonimo della classe padre.
Nel costruttore serve per inizializzare gli attributi ereditati senza
riscriverli.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire. `Docente` e `Alunno`
condividono nome, cognome e il metodo `saluta` — definiti una sola volta
nella classe padre `Persona`.

{% highlight ruby %}
class Persona
  def initialize(nome, cognome)
    @nome = nome
    @cognome = cognome
  end

  def saluta
    "Ciao, mi chiamo #{@nome} #{@cognome}"
  end
end

class Docente < Persona
  def initialize(nome, cognome, materia)
    super(nome, cognome)
    @materia = materia
  end

  def lavora
    "Sto insegnando #{@materia}"
  end
end

class Alunno < Persona
  def initialize(nome, cognome, classe)
    super(nome, cognome)
    @classe = classe
  end

  def lavora
    "Sto frequentando la classe #{@classe}"
  end
end

# main
mario   = Alunno.new("Mario", "Rossi", "4C")
rosa    = Alunno.new("Rosa", "Verdi", "3C")
armando = Docente.new("Armando", "Bianchi", "Filosofia")

puts mario.saluta
puts mario.lavora
puts rosa.saluta
puts rosa.lavora
puts armando.saluta
puts armando.lavora
{% endhighlight %}

## Polimorfismo

Si parla di **polimorfismo** quando oggetti di classi diverse rispondono
allo stesso messaggio (stesso nome di metodo) producendo comportamenti
diversi. Questo permette di scrivere codice che lavora su oggetti di tipo
diverso in modo uniforme.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire. Tutte le figure
geometriche rispondono al metodo `calcola_area` ma ognuna lo calcola in
modo diverso.

{% highlight ruby %}
class Rettangolo
  def initialize(base, altezza)
    @base = base
    @altezza = altezza
  end

  def calcola_area
    @base * @altezza
  end
end

class Triangolo
  def initialize(base, altezza)
    @base = base
    @altezza = altezza
  end

  def calcola_area
    @base * @altezza / 2.0
  end
end

class Cerchio
  def initialize(raggio)
    @raggio = raggio
  end

  def calcola_area
    3.14 * @raggio ** 2
  end
end

# main
figure = [Rettangolo.new(4, 5), Triangolo.new(4, 5), Cerchio.new(3)]
for figura in figure
  puts figura.calcola_area
end
{% endhighlight %}

Il ciclo `for` non sa di che tipo è ogni oggetto: sa solo che tutti
rispondono a `calcola_area`. Questo è il vantaggio del polimorfismo.

## Composizione

La **composizione** consiste nel definire attributi di una classe come
istanze di un'altra classe. Si usa quando un oggetto è composto da altri
oggetti.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire. Un `Appartamento`
è composto da più `Stanza`.

{% highlight ruby %}
class Stanza
  def initialize(nome, lunghezza, larghezza)
    @nome = nome
    @lunghezza = lunghezza
    @larghezza = larghezza
  end

  def calcola_superficie
    @lunghezza * @larghezza
  end

  def nome
    @nome
  end
end

class Appartamento
  def initialize(indirizzo)
    @indirizzo = indirizzo
    @stanze = []
  end

  def aggiungi_stanza(stanza)
    @stanze << stanza
  end

  def calcola_superficie
    totale = 0
    for stanza in @stanze
      totale = totale + stanza.calcola_superficie
    end
    totale
  end
end

# main
appartamento = Appartamento.new("Via Mazzini, 22")
appartamento.aggiungi_stanza(Stanza.new("Camera grande", 5, 5))
appartamento.aggiungi_stanza(Stanza.new("Camera piccola", 4, 5))
appartamento.aggiungi_stanza(Stanza.new("Bagno", 4, 2))
appartamento.aggiungi_stanza(Stanza.new("Cucina", 4, 3))
appartamento.aggiungi_stanza(Stanza.new("Salotto", 6, 4))

puts "Superficie totale: #{appartamento.calcola_superficie} m²"
{% endhighlight %}

---

## Esercizi

#### Esercizio 7
Crea una classe `Impiegato` con gli attributi `matricola`, `nome`, `salario`
e `dipartimento`. Aggiungi i metodi:
- `descrivi` — restituisce una stringa con tutti i dati
- `calcola_stipendio(ore)` — calcola lo stipendio: fino a 40 ore è `ore * salario / 40`; le ore eccedenti sono pagate il 50% in più

Crea almeno tre istanze e chiama entrambi i metodi su ciascuna.

#### Esercizio 8
Crea una classe `Libro` con gli attributi `isbn`, `titolo`, `autore`,
`pagine`. Aggiungi il metodo `presentazione` che restituisce la stringa
`"Il libro <titolo> è stato scritto da <autore>"`.
Crea 4 istanze, inseriscile in un array e stampa la presentazione di
ciascuna con un ciclo.

#### Esercizio 9
Crea una classe `Conto` con l'attributo `saldo` inizializzato a 0. Aggiungi
i metodi `deposita(ammontare)`, `preleva(ammontare)` (che rifiuta se il
saldo è insufficiente) e `estratto_conto`. Crea due conti distinti e
simula alcune operazioni, stampando il saldo dopo ogni operazione.

#### Esercizio 10
Crea una classe `Automobile` con gli attributi `modello` e `km_percorsi`
(inizializzato a 0). Aggiungi i metodi:
- `percorri(km)` — aggiunge i km all'odometro
- `km_totali` — restituisce i km percorsi
Crea due istanze e simula alcuni viaggi.

#### Esercizio 11
Usando l'ereditarietà, crea la gerarchia `Animale` → `Mammifero`, `Rettile`,
`Uccello`. Ogni classe figlia aggiunge un metodo `verso` che restituisce il
suono dell'animale. Crea almeno due istanze per ciascuna sottoclasse e
raccoglile in un array; stampa il verso di tutti con un ciclo.

#### Esercizio 12
Crea le classi `Albero` (attributi: `nome`, `altezza`) e `Foresta`
(contiene una lista di alberi). Aggiungi a `Foresta` i metodi
`aggiungi_albero`, `albero_piu_alto` e `altezza_totale`. Crea una foresta
con almeno 5 alberi e chiama tutti i metodi.
