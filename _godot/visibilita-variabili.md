---
title: 'Godot: visibilità delle variabili'
date: '2026-08-18T10:05:00+01:00'
author: Fabio Mattei
layout: page
---

## Visibilità delle variabili

A differenza di Ruby, dove un prefisso diverso (`@`, `@@`, `$`) rende immediatamente
riconoscibile lo scope di una variabile dal suo nome, in GDScript lo scope dipende
**da dove la variabile è dichiarata**, non da come si chiama.

{% highlight gdscript %}
extends Node

var attributo_di_istanza = 0    # dichiarata a livello di script: un attributo
static var contatore_condiviso = 0    # condivisa da tutte le istanze dello script

func _ready():
    var variabile_locale = 10    # dichiarata dentro una funzione: solo locale
{% endhighlight %}

---

## Variabili locali

Una variabile dichiarata con `var` **dentro** una funzione esiste solo mentre quella
funzione è in esecuzione, esattamente come in Ruby.

{% highlight gdscript %}
func saluta():
    var messaggio = "Ciao!"
    print(messaggio)

func _ready():
    saluta()
    # print(messaggio)   # errore: messaggio non esiste qui fuori
{% endhighlight %}

---

## Attributi di istanza (member variables)

Una variabile dichiarata con `var` **fuori** da qualsiasi funzione, a livello dello
script, è un **attributo di istanza**: appartiene al singolo oggetto (nodo) a cui lo
script è collegato, ed è visibile da tutte le funzioni dello script.

{% highlight gdscript %}
extends Node

var vita = 100    # attributo di istanza

func danneggia(quantita):
    vita -= quantita

func _ready():
    danneggia(30)
    print(vita)    # 70
{% endhighlight %}

Se lo stesso script viene attaccato a due nodi diversi, ciascun nodo avrà la
**propria copia** di `vita`, indipendente dall'altra — è l'equivalente
dell'`@vita` di Ruby.

---

## Variabili statiche (equivalenti a @@ di Ruby)

Una variabile dichiarata con `static var` è **condivisa da tutte le istanze** dello
stesso script: modificarla da un'istanza la cambia per tutte, proprio come `@@` in
Ruby.

{% highlight gdscript %}
extends Node

static var nemici_creati = 0

func _init():
    nemici_creati += 1

func _ready():
    print(nemici_creati)
{% endhighlight %}

Se questo script fosse la classe `Nemico`, ogni nuovo nemico creato incrementerebbe
lo stesso contatore condiviso, indipendentemente da quale istanza lo faccia.

---

## Costanti

`const` dichiara un valore che **non può essere riassegnato**. È più rigido delle
costanti Ruby (che sono solo una convenzione): tentare di modificare una `const` è
un errore rilevato da Godot prima ancora di eseguire lo script.

{% highlight gdscript %}
const GRAVITA = 9.81
const VELOCITA_MASSIMA = 300
{% endhighlight %}

---

## Variabili globali: gli Autoload

Ruby ha le variabili globali con il prefisso `$`, accessibili da ovunque nel
programma. In Godot il concetto equivalente si chiama **Autoload** (o
**singleton**): uno script speciale, registrato nelle impostazioni del progetto
(**Project > Project Settings > Autoload**), le cui variabili sono accessibili da
qualsiasi altro script del gioco usando direttamente il suo nome.

{% highlight gdscript %}
# script GestoreGioco.gd, registrato come autoload "GestoreGioco"
extends Node

var punteggio = 0
var record = 0
{% endhighlight %}

{% highlight gdscript %}
# in un qualsiasi altro script del progetto
func _ready():
    GestoreGioco.punteggio += 10
    print(GestoreGioco.punteggio)
{% endhighlight %}

Come per le variabili globali di Ruby, gli Autoload vanno usati con parsimonia: sono
comodi per dati che riguardano davvero tutto il gioco (il punteggio, lo stato della
partita), ma se ne fa un uso eccessivo il codice diventa difficile da seguire.
Ritroveremo questa tecnica nella pagina su [stato del gioco e cambio scena]({{ site.baseurl }}{% link _godot/scene.md %}.html).

---

## Riepilogo: da Ruby a GDScript

| Ruby                    | GDScript equivalente                    |
|--------------------------|--------------------------------------------|
| variabile locale         | `var` dentro una funzione                   |
| `@variabile_di_istanza`  | `var` a livello di script                   |
| `@@variabile_di_classe`  | `static var` a livello di script            |
| `$variabile_globale`     | attributo di un nodo **Autoload**           |
| `COSTANTE`               | `const`                                     |
