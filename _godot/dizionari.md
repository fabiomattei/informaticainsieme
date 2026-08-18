---
title: 'Godot: i dizionari'
date: '2026-08-18T09:40:00+01:00'
author: Fabio Mattei
layout: page
---

## Chiavi e valori

Se gli array sono cassettiere con i cassetti numerati, i **Dictionary** (dizionari)
sono cassettiere con i cassetti **etichettati**. Un dizionario associa ogni
**valore** a una **chiave** univoca che lo identifica: è l'equivalente GDScript
dell'hash di Ruby.

Le chiavi possono essere stringhe, numeri o quasi qualsiasi altro valore, e non
devono essere tutte dello stesso tipo.

---

## Creare un dizionario

#### Esercizio 1
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var vuoto = {}
    var persona = {"nome": "Mario", "cognome": "Rossi", "eta": 25}

    print(persona["nome"])     # Mario
    print(persona["cognome"])  # Rossi
    print(persona["eta"])      # 25
    print(persona.size())      # 3
{% endhighlight %}

GDScript accetta anche una sintassi alternativa in stile Lua, valida quando le
chiavi sono identificatori (senza spazi, senza iniziare con un numero):

{% highlight gdscript %}
var h1 = {"nome": "Mario"}   # sintassi con stringa (sempre valida)
var h2 = {nome = "Mario"}    # sintassi Lua-like (solo per chiavi identificatore)

print(h1["nome"])    # accesso sempre possibile con le parentesi
print(h2.nome)        # con questa sintassi si può anche accedere col punto
{% endhighlight %}

---

## Accedere agli elementi

Si usa la notazione `dizionario[chiave]`. A differenza di Ruby, dove una chiave
inesistente restituisce silenziosamente `nil`, in GDScript **genera un errore a
runtime** se la chiave non esiste. Per un accesso sicuro si usa `.get()`.

#### Esercizio 2
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var voti = {"fisica": 8, "matematica": 6, "storia": 7, "inglese": 9}

    print(voti["fisica"])            # 8
    print(voti.get("storia"))        # 7
    print(voti.get("arte"))          # null  (chiave inesistente, nessun errore)
    print(voti.get("arte", 0))       # 0     (valore di default esplicito)
{% endhighlight %}

---

## Aggiungere, modificare e rimuovere

#### Esercizio 3
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var persona = {"nome": "Mario", "eta": 25}

    # aggiungere una nuova chiave
    persona["citta"] = "Roma"
    print(persona)    # {nome: Mario, eta: 25, citta: Roma}

    # modificare un valore esistente
    persona["eta"] = 26
    print(persona["eta"])    # 26

    # rimuovere una chiave
    persona.erase("citta")
    print(persona)    # {nome: Mario, eta: 26}
{% endhighlight %}

---

## Verificare la presenza di una chiave

#### Esercizio 4
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var voti = {"fisica": 8, "matematica": 6, "storia": 7}

    if voti.has("fisica"):
        print("La fisica è presente")

    if not voti.has("arte"):
        print("L'arte non è presente")

    # equivalente più corto, con l'operatore in
    if "fisica" in voti:
        print("La fisica è presente (con in)")
{% endhighlight %}

---

## Visitare un dizionario

#### Esercizio 5
Copia il seguente codice dentro `_ready()` e fallo eseguire. Come Ruby, iterare
direttamente su un dizionario con `for` scorre le sue **chiavi**.

{% highlight gdscript %}
func _ready():
    var stati_e_capitali = {
        "italia": "Roma",
        "francia": "Parigi",
        "germania": "Berlino",
        "spagna": "Madrid"
    }

    for stato in stati_e_capitali:
        print("%s: %s" % [stato, stati_e_capitali[stato]])
{% endhighlight %}

Si possono anche visitare solo le chiavi o solo i valori:

{% highlight gdscript %}
func _ready():
    var voti = {"fisica": 8, "matematica": 6}
    print(voti.keys())      # [fisica, matematica]
    print(voti.values())    # [8, 6]
{% endhighlight %}

---

## Metodi utili

| Metodo              | Significato                                        |
|----------------------|-------------------------------------------------------|
| `.size()`            | numero di coppie chiave/valore                        |
| `.keys()`            | array di tutte le chiavi                              |
| `.values()`          | array di tutti i valori                               |
| `.has(k)`            | `true` se la chiave `k` esiste                        |
| `.has_all([k1, k2])` | `true` se tutte le chiavi indicate esistono           |
| `.erase(k)`          | rimuove la coppia con chiave `k`                      |
| `.merge(altro)`      | unisce due dizionari (per default non sovrascrive)    |
| `.duplicate()`       | crea una copia indipendente del dizionario            |

#### Esercizio 6
Copia il seguente codice dentro `_ready()` e fallo eseguire.

{% highlight gdscript %}
func _ready():
    var voti = {"fisica": 8, "matematica": 6, "storia": 7, "inglese": 9}

    var extra = {"educazione_fisica": 9, "arte": 7}
    var completo = voti.duplicate()
    completo.merge(extra)
    print(completo.size())    # 6
{% endhighlight %}

`.merge()` per default **non sovrascrive** le chiavi già presenti: per farlo va
passato `true` come secondo argomento, `voti.merge(extra, true)`.

---

## Esercizi

#### Esercizio 7
Definisci un dizionario `persona` con le chiavi `nome`, `cognome` e `indirizzo`.
Scrivi un ciclo che stampi tutte le coppie chiave/valore.

#### Esercizio 8
Scrivi uno script che generi un dizionario in cui le chiavi sono i numeri da 1 a 5
(con `range()`) e i valori sono i loro quadrati.

#### Esercizio 9
Dato un dizionario `voti` (materia → voto), scrivi uno script che stampi il nome
della materia con il voto più alto e quello con il voto più basso, scorrendo le
chiavi con un ciclo `for`.

#### Esercizio 10
Crea un array di dizionari, ciascuno che descrive uno studente con `nome` e
`voto_medio`. Scrivi un ciclo che stampi solo gli studenti con voto medio maggiore o
uguale a 7.
