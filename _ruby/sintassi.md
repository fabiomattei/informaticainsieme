---
title: 'Sintassi del linguaggio Ruby'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---


## Tipi di dato

Ruby definisce numerosi tipi di dato, alcuni sono i soliti: stringa, intero, float, boolean

Altri sono le costanti, riconoscibili perché sempre maiuscole e danno un warning se modificate
i simboli, che sono stringe di testo più leggere e con meno funzionalità ma molto più veloci.

In fine abbiamo gli array, corrispondenti alle liste in python e gli hash corrispondendi ai
dizionari in python.


{% highlight ruby %}
s = "string"
s = 'also string'
i = 100
f = 3.14
ofcourse = true
CON = 'constant' # warning if modified
:symbol
none = nil
array = [i, f, s, {:symbol → 'lol'}, CON] 
hash = { :5 => 42, 'key': 'value', anything: ar }
{% endhighlight %}

Gli hash sono interessanti perché i dati vi possono essere inseriti in tre modi diversi:

* :5 => 42           # simbolo => valore
* 'key': 'value'     # 'chiave': 'valore
* anything: ar       # simbolo: valore

## Operatori

### Assegnamento

Vediamo tutti gli operatori di assegnamento. Il primo è il normale assegnamento con simblo **=**. 
A seguire vanno gli operatori di assegnamento che includono al loro interno una operazione artimetica.
Ad esempio c += a equivale a scrivere **c = c + a**

{% highlight ruby %}
c = a + b
c += a
c -= a
c *= a
c /= a 
c %= a
c **= a
{% endhighlight %}

DEFINED? VAR_NAME # si usa per capire se una variabili è stata definita

### Assegnamento parallelo

{% highlight ruby %}
a, b = 1, 2

a, b = [1,3] # spacchetta

a,b = [1,2,3,4] # il resto viene ignorato

a, b, *c = [1, 3, 4, 5] # c = [4, 5] # operatore splat 
{% endhighlight %}

### Scambio

L'assegnamento parallelo è comodo se devo scambiare tra loro i valori di due variabili.

{% highlight ruby %}
a, b = b, a
{% endhighlight %}

### Aritmetici

{% highlight ruby %}
3 + 2
3 - 2
3 * 2
3 / 2    # risulta 1
3 / 2.0  # risulta 1.5
3 ** 2   # risulta 9
3 % 2    # risulta 1
{% endhighlight %}

### Logici

{% highlight ruby %}
true and false 
true && false   # equivale al precedente
true or false 
true || false   # equivale al precedente
not true 
! true          # equivale al precedente
{% endhighlight %}

### Confronto

{% highlight ruby %}
3 == 4
3 < 4
3 > 4
4 >= 4
4 <= 4
{% endhighlight %}

### Confronto  con operatore spaceship (nave spaziale)

E' un operatore che ha un comportamento un po' strano ma può risultare molto
utile. L'operature restituisce:

* -1: il numero sinistro è più piccolo del destro
* 0: i numeri sono uguali
* 1: il numero destro è più piccolo del sinistro

{% highlight ruby %}
3 <=> 4 #  -1(sinistro più piccolo), 0 (uguale), 1 (destro più piccolo)
{% endhighlight %}

### Operatore ternario

E' un operatore che inizia con una espressione booleana seguita da un punto interrogativo.
Restituisce il valore che segue il punto interrogativo se l'espressione booleana è vera, 
restituisce il valore che segue i due punti se l'espressione booleana è falsa. 

{% highlight ruby %}
a = 10 ? 1 : 3
{% endhighlight %}

### Operatore range 

{% highlight ruby %}
1 .. 3     # 1,2,3 ; limite destro incluso
1 ... 3    # 1,2 limite destro escluso
{% endhighlight %}

## Visibilità delle variabili

Il linguaggio Ruby rende la visibilità delle variabili immediatamente riconoscibile.

{% highlight ruby %}
una_semplice_variabile
@variabile_di_istanza 
@@variabile_di_classe
$variabile_globale
COSTANTE
{% endhighlight %}


## Condizioni

Il linguaggio Ruby permette di scrivere le condizioni in tre modi diversi.

Il primo è il modo normale a cui siamo abituati: if seguito da espressione booleana,
tanti elsif seguiti da espressione booleana quanto ne abbiamo bisogno, else finale. 
Il blocco si chiude con la parola end.

{% highlight ruby %}
if true 
   puts 'if' 
elsif false
   puts 'else-if' 
else 
   puts 'else'
end
{% endhighlight %}

Il linguaggio Ruby è l'unico che dispone della parola chiave **unless** traducibile 
in **if not**. 

{% highlight ruby %}
unless false 
   puts 'if not'
elsif true
   puts 'else if'
else
   puts 'else'
end
{% endhighlight %}

In fine possiamo utilizzare l'istruzione case. In questo esempio leggiamo la variabile
voto dall'input e trasformiamo il numero in intero.
A questo punto si procede per range di valori:

* numero letto è compreso tra 0 e 9: il computer scrive "voto bassissimo"
* 10: il computer scrive '10!!!!'
* 100: il computer scrive "WOW!!!!"
* 80, 90 o 95: il computer scrive  'numeri buoni'
* per tutti gli altri numeri: il computer scrive 'va bene...'


{% highlight ruby %}
voto = gets.chomp.to_i # int(input().strip())
case voto
   when 0..9
      puts 'voto bassissimo'
   when 10 then puts '10!!!!' 
   when 100
      puts "WOW!!!!"
   when 80,90, 95
      puts 'numeri buoni'
   else
      puts 'va bene...'
end
{% endhighlight %}


## Cicli

### ciclo while

{% highlight ruby %}
while gets.chomp != 'stop' 
    puts 'wont stop'
end
{% endhighlight %}

### ciclio while con condizione in negativo
{% highlight ruby %}
until gets.chomp == 'stop'
    puts 'wont stop'
end
{% endhighlight %}

### ciclo do-while 
{% highlight ruby %}
begin
    puts 'do'
end while true
{% endhighlight %}

### ciclo for
{% highlight ruby %}
for i in 1.. 10
    if i = 2
       redo
    elsif i = 3
       next
    elsif i = 4
       break
    end 
end
{% endhighlight %}

### ciclo for per visitare un array (lista)
{% highlight ruby %}
for i in [1, 3, 4]
    puts i
end
{% endhighlight %}

### ciclo for per visitare un hash (dizionario)
{% highlight ruby %}
for k,v in {a: 1, b: 2} 
    puts k, v 
end
{% endhighlight %}


## Funzioni

In ruby una funzione è definita usando la parola chiave def e chiudendo il corpo della funzione
con la parola end.

{% highlight ruby %}
def somma(a, b)
    return a + b
end
{% endhighlight %}

Le parentesi non sono obbligatorie

{% highlight ruby %}
def somma a, b
    return a + b
end
{% endhighlight %}

Il return può essere implicito, se non diversamente specificato Ruby restituisce al chiamate
il risultato dell'ultima espressione valutata.

{% highlight ruby %}
def somma a, b
    a + b
end
{% endhighlight %}

In ruby le funzioni possono avere parametri facoltativi

{% highlight ruby %}
def somma a, b, c = 2
    a + b + c
end
{% endhighlight %}

Ruby è in grado poi di raggruppare i parametri ricevuti in liste oppure in dizionari:

{% highlight ruby %}
def midiverto(a, b, c = 10, *args, **kwargs)
    # kwargs = {}
    # args = []
    if c = 3
        return 0
    a + b + c # return implicito
end

x = midiverto 1, 2, 3, 4, 5, m: 100
{% endhighlight %}

In questo caso i parametri vengono assegnati in questo modo:

* a = 1
* b = 2
* c = 3
* args = [ 4, 5 ]
* kwargs = { m: 100 }

## Classi

Defianiamo una classe Cerchio contenente l'attributo di istanza raggio e l'attributo di classe
pigreco. Definiamo poi il costruttore initialize e il metodo calcola_area.
In fine creiamo una istanza e chiamiamo il metodo calcola_area.

{% highlight ruby %}
class Cerchio
   @@pigreco = 3.14
   
   def initialize(raggio)
       @raggio = raggio
   end
   
   def calcola_area
       @raggio ** 2 * @@pigreco
   end 
   
end

# main
cerchio = Cerchio.new(4)
puts cerchio.calcola_area
{% endhighlight %}

### Ereditarietà

La dindenza di ereditarietà tra classi si esprime con il simbolo **<**.

{% highlight ruby %}
class Persona
   def initialize(nome, cognome)
       @nome = nome
       @cognome = cognome
   end
   
   def saluta
       nome + ' ' + cognome
   end 
end

class Studente < Persona
   def initialize(nome, cognome, scuola)
       super(nome, cognome)
       @scuola = scuola
   end
   
   def saluta
       @nome + ' ' + @cognome + ' frequento: ' + @scuola
   end 
end


# main
mario = Stutente.new('Mario', 'Rossi', 'Patini')
puts mario.saluta
{% endhighlight %}

