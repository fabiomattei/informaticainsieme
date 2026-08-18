---
title: 'Godot: classi'
date: '2026-08-18T10:35:00+01:00'
author: Fabio Mattei
layout: page
---

## Introduzione

Come visto per Ruby, la **programmazione ad oggetti** raggruppa insieme le
informazioni (**attributi**) e gli algoritmi che le riguardano (**metodi**)
all'interno di un'unica struttura chiamata **classe**.

In Godot ogni script **è già di per sé una classe**: quando si scrive
`extends Node`, si sta definendo una nuova classe che eredita da `Node`. Non serve
una parola chiave `class` per lo script principale — serve solo per le classi
**interne** allo script, come visto nella pagina sulle
[strutture dati leggere]({{ site.baseurl }}{% link _godot/struct.md %}.html).

## La prima classe

Per dare un nome a una classe, utilizzabile da altri script senza dover conoscere
il percorso del file, si usa `class_name`.

#### Esercizio 1
Crea un nuovo script chiamato `cerchio.gd` con questo contenuto, poi richiamalo da
un altro script (ad esempio quello attaccato al nodo principale della scena) come
nell'esempio sotto.

{% highlight gdscript %}
# cerchio.gd
class_name Cerchio
extends RefCounted

const PIGRECO = 3.14

var raggio

func _init(nuovo_raggio):
    raggio = nuovo_raggio

func calcola_area():
    return pow(raggio, 2) * PIGRECO

func calcola_circonferenza():
    return 2 * PIGRECO * raggio
{% endhighlight %}

{% highlight gdscript %}
# script attaccato a un nodo della scena
func _ready():
    var cerchio = Cerchio.new(5)
    print(cerchio.calcola_area())
    print(cerchio.calcola_circonferenza())
{% endhighlight %}

`extends RefCounted` indica che `Cerchio` è un oggetto "puro", che non fa parte
dell'albero della scena (non è un `Node` da disegnare): è la scelta giusta per
classi che rappresentano solo dati e logica, come farebbe una classe Ruby qualsiasi.

## Attributi di istanza e costanti

Gli attributi si distinguono in base a **dove** e **come** sono dichiarati, non dal
loro nome (vedi la pagina sulla [visibilità delle variabili]({{ site.baseurl }}{% link _godot/visibilita-variabili.md %}.html)):

| Dichiarazione | Tipo             | Condiviso tra istanze? |
|-----------------|------------------|--------------------------|
| `var`           | attributo di istanza | no — ogni oggetto ha il suo valore |
| `static var`    | attributo di classe  | sì — uguale per tutti gli oggetti |
| `const`         | costante             | sì, e non modificabile |

`_init()` è il **costruttore**: viene chiamato automaticamente da Godot quando si
crea una nuova istanza con `.new()`. I parametri passati a `.new()` arrivano
direttamente a `_init()` — è l'equivalente esatto dell'`initialize` di Ruby.

## Classi e istanze

#### Esercizio 2
Crea uno script `alunno.gd` e richiamalo come mostrato.

{% highlight gdscript %}
# alunno.gd
class_name Alunno
extends RefCounted

var nome
var cognome

func _init(n, c):
    nome = n
    cognome = c

func saluta():
    return "Ciao, mi chiamo %s %s" % [nome, cognome]
{% endhighlight %}

{% highlight gdscript %}
func _ready():
    var mario = Alunno.new("Mario", "Rossi")
    var rita = Alunno.new("Rita", "Morelli")

    print(mario.saluta())
    print(rita.saluta())
{% endhighlight %}

`mario` e `rita` sono due istanze distinte della stessa classe `Alunno`: condividono
la struttura (attributi e metodi) ma ognuna ha i propri valori.

## Niente attr_accessor

In GDScript i campi dichiarati con `var` sono **già pubblici e accessibili
dall'esterno per default** (`mario.nome`): non serve nessun equivalente di
`attr_reader` o `attr_accessor`. Per rendere un attributo di sola lettura si usa la
convenzione del prefisso underscore (`_nome`, solo una convenzione, non imposta dal
linguaggio) oppure si espone un metodo dedicato invece dell'attributo diretto.

{% highlight gdscript %}
class_name Alunno
extends RefCounted

var nome
var cognome
var voto_medio = 0

func _init(n, c):
    nome = n
    cognome = c

func imposta_voto(v):
    voto_medio = v
{% endhighlight %}

## Ereditarietà

La **ereditarietà** permette di creare una nuova classe che estende una classe
esistente. Si usa `extends` per indicare la classe padre (proprio come si usa per
ereditare da `Node`, `RefCounted` o qualsiasi altro tipo di Godot), e `super()` per
richiamare il metodo omonimo della classe padre.

#### Esercizio 3
Crea tre script: `persona.gd`, `docente.gd` e `alunno_scuola.gd`.

{% highlight gdscript %}
# persona.gd
class_name Persona
extends RefCounted

var nome
var cognome

func _init(n, c):
    nome = n
    cognome = c

func saluta():
    return "Ciao, mi chiamo %s %s" % [nome, cognome]
{% endhighlight %}

{% highlight gdscript %}
# docente.gd
class_name Docente
extends Persona

var materia

func _init(n, c, m):
    super(n, c)
    materia = m

func lavora():
    return "Sto insegnando %s" % materia
{% endhighlight %}

{% highlight gdscript %}
# alunno_scuola.gd
class_name AlunnoScuola
extends Persona

var classe

func _init(n, c, cl):
    super(n, c)
    classe = cl

func lavora():
    return "Sto frequentando la classe %s" % classe
{% endhighlight %}

{% highlight gdscript %}
func _ready():
    var mario = AlunnoScuola.new("Mario", "Rossi", "4C")
    var armando = Docente.new("Armando", "Bianchi", "Filosofia")

    print(mario.saluta())
    print(mario.lavora())
    print(armando.saluta())
    print(armando.lavora())
{% endhighlight %}

`super(n, c)` chiama l'`_init` della classe padre `Persona`, inizializzando `nome` e
`cognome` senza doverli riscrivere.

## Polimorfismo

Si parla di **polimorfismo** quando oggetti di classi diverse rispondono allo stesso
messaggio (stesso nome di metodo) producendo comportamenti diversi.

#### Esercizio 4
Crea tre classi interne allo stesso script e mettile in un array, come nell'esempio
seguente.

{% highlight gdscript %}
class Rettangolo:
    var base
    var altezza
    func _init(b, a):
        base = b
        altezza = a
    func calcola_area():
        return base * altezza

class Triangolo:
    var base
    var altezza
    func _init(b, a):
        base = b
        altezza = a
    func calcola_area():
        return base * altezza / 2.0

class Cerchio:
    var raggio
    func _init(r):
        raggio = r
    func calcola_area():
        return 3.14 * pow(raggio, 2)

func _ready():
    var figure = [Rettangolo.new(4, 5), Triangolo.new(4, 5), Cerchio.new(3)]
    for figura in figure:
        print(figura.calcola_area())
{% endhighlight %}

Il ciclo `for` non sa di che tipo è ogni oggetto: sa solo che tutti rispondono a
`calcola_area()`. Questo è il vantaggio del polimorfismo, identico in GDScript e in
Ruby.

## Composizione

La **composizione** consiste nel definire attributi di una classe come istanze di
un'altra classe.

#### Esercizio 5
Copia il seguente codice nello script e fallo eseguire. Un `Appartamento` è
composto da più `Stanza`.

{% highlight gdscript %}
class Stanza:
    var nome
    var lunghezza
    var larghezza
    func _init(n, l, la):
        nome = n
        lunghezza = l
        larghezza = la
    func calcola_superficie():
        return lunghezza * larghezza

class Appartamento:
    var indirizzo
    var stanze = []
    func _init(ind):
        indirizzo = ind
    func aggiungi_stanza(stanza):
        stanze.append(stanza)
    func calcola_superficie():
        var totale = 0
        for stanza in stanze:
            totale += stanza.calcola_superficie()
        return totale

func _ready():
    var appartamento = Appartamento.new("Via Mazzini, 22")
    appartamento.aggiungi_stanza(Stanza.new("Camera grande", 5, 5))
    appartamento.aggiungi_stanza(Stanza.new("Camera piccola", 4, 5))
    appartamento.aggiungi_stanza(Stanza.new("Bagno", 4, 2))
    appartamento.aggiungi_stanza(Stanza.new("Cucina", 4, 3))

    print("Superficie totale: %s m²" % appartamento.calcola_superficie())
{% endhighlight %}

---

## Classi che sono anche nodi

Tutte le classi viste finora estendono `RefCounted`: oggetti puri di dati e logica,
senza posizione né grafica. Nella prossima sezione, dedicata alla costruzione di un
vero gioco, le classi estenderanno quasi sempre `Node2D` o una delle sue
sottoclassi (`Sprite2D`, `Area2D`, …): in quel caso l'istanza non si crea più con
`.new()` dentro allo script, ma **collegando lo script a un nodo** della scena
tramite l'editor, come vedremo nella pagina su
[interfaccia e primo script]({{ site.baseurl }}{% link _godot/introduzione.md %}.html).

---

## Esercizi

#### Esercizio 6
Crea una classe `Impiegato` con gli attributi `matricola`, `nome`, `salario` e
`dipartimento`. Aggiungi i metodi `descrivi()` e `calcola_stipendio(ore)` (fino a 40
ore `ore * salario / 40`; oltre, le ore eccedenti pagate il 50% in più).

#### Esercizio 7
Crea una classe `Conto` con l'attributo `saldo` inizializzato a 0. Aggiungi i metodi
`deposita(ammontare)`, `preleva(ammontare)` (che rifiuta se il saldo è
insufficiente) ed `estratto_conto()`.

#### Esercizio 8
Usando l'ereditarietà, crea la gerarchia `Animale` → `Mammifero`, `Rettile`,
`Uccello`. Ogni classe figlia aggiunge un metodo `verso()` che restituisce il suono
dell'animale. Crea almeno due istanze per ciascuna sottoclasse, raccoglile in un
array e stampa il verso di tutti con un ciclo.
