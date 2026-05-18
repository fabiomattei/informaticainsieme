---
title: 'Ruby: le stringhe di testo'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Una sequenza di caratteri

Una stringa di testo è una **sequenza ordinata di caratteri**. Il computer
non conosce le lettere direttamente: usa la tabella ASCII per associare un
numero intero a ciascun simbolo. La parola "CIAO" viene memorizzata come
quattro numeri in sequenza:

| C  | I  | A  | O  |
|----|----|----|-----|
| 67 | 73 | 65 | 79 |

Per questo le stringhe sono dette **tipi composti**: sono formate da unità
più piccole (i caratteri) disposte in un ordine preciso.

---

## Inizializzare una stringa

In Ruby le stringhe si delimitano con **apici singoli** `'...'` o
**apici doppi** `"..."`. La differenza fondamentale è che gli apici doppi
attivano l'**interpolazione**: il contenuto di una variabile o di
un'espressione può essere inserito direttamente nella stringa con `#{}`.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
nome = "Ruby"
puts "Benvenuto in #{nome}!"    # Benvenuto in Ruby!
puts 'Benvenuto in #{nome}!'    # Benvenuto in #{nome}!  (nessuna interpolazione)
puts "2 + 2 = #{2 + 2}"        # 2 + 2 = 4
{% endhighlight %}

Con gli apici singoli il testo viene trattato letteralmente. Con gli apici
doppi tutto ciò che si trova dentro `#{}` viene valutato come espressione
Ruby e il risultato viene inserito nella stringa.

---

## Lunghezza di una stringa

Il metodo `.length` (o il sinonimo `.size`) restituisce il numero di
caratteri contenuti nella stringa.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
saluto = "ciao"
puts saluto.length     # 4

parola = "informatica"
puts parola.length     # 11

puts "".length         # 0  (stringa vuota)
{% endhighlight %}

---

## Accedere a un singolo carattere

Ruby numera ogni carattere con un **indice** che parte da 0. Il carattere
all'indice `i` si ottiene con `stringa[i]`. Gli indici negativi contano
dalla fine: `-1` è l'ultimo carattere, `-2` il penultimo e così via.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
saluto = "ciao"
puts saluto[0]    # c
puts saluto[1]    # i
puts saluto[3]    # o
puts saluto[-1]   # o  (ultimo carattere)
puts saluto[-2]   # a  (penultimo)
{% endhighlight %}

| lettera | c | i | a | o |
|---------|---|---|---|---|
| indice  | 0 | 1 | 2 | 3 |
| indice negativo | -4 | -3 | -2 | -1 |

---

## Sottostringhe (slicing)

Con la notazione `stringa[inizio..fine]` si estrae una porzione di stringa.
Il range `..` include l'estremo finale; `...` lo esclude.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
parola = "informatica"
puts parola[0..4]    # infor
puts parola[5..11]   # matica
puts parola[0...4]   # info  (esclude l'indice 4)
puts parola[-5..-1]  # atica (ultimi 5 caratteri)
{% endhighlight %}

---

## Confrontare le stringhe

Le stringhe si confrontano con gli stessi operatori dei numeri. Il confronto
avviene carattere per carattere seguendo l'ordine della tabella ASCII:
le lettere maiuscole precedono le minuscole.

| Operatore | Significato        |
|-----------|--------------------|
| `==`      | uguale             |
| `!=`      | diverso            |
| `<`       | viene prima (alfabeticamente) |
| `>`       | viene dopo         |
| `<=`      | prima o uguale     |
| `>=`      | dopo o uguale      |

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
puts "ciao" == "ciao"      # true
puts "mattino" > "sera"    # false  (m viene prima di s)
puts "mattino" > "mattina" # true   (o viene dopo a)
puts "A" < "a"             # true   (maiuscole prima di minuscole)

a = "banana"
b = "arancia"
if a < b
  puts a
else
  puts b
end
{% endhighlight %}

---

## Metodi sulle stringhe

Ruby offre molti metodi per trasformare e interrogare le stringhe.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
s = "Ciao Mondo"
puts s.upcase        # CIAO MONDO
puts s.downcase      # ciao mondo
puts s.reverse       # odnoM oaiC
puts s.length        # 10
puts s.include?("Mondo")   # true
puts s.include?("mondo")   # false  (maiuscole contano)
{% endhighlight %}

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire. `gsub` sostituisce
tutte le occorrenze di un testo con un altro.

{% highlight ruby %}
frase = "il gatto sul tetto"
puts frase.gsub("t", "T")       # il gaTTo sul TeTTo
puts frase.gsub("tetto", "prato") # il gatto sul prato
{% endhighlight %}

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire. `split` divide una
stringa in un array di sottostringhe.

{% highlight ruby %}
frase = "uno due tre quattro"
parole = frase.split(" ")
puts parole.length    # 4
for parola in parole
  puts parola
end
{% endhighlight %}

Tabella riepilogativa dei metodi principali:

| Metodo              | Significato                                    |
|---------------------|------------------------------------------------|
| `.length`           | numero di caratteri                            |
| `.upcase`           | tutto maiuscolo                                |
| `.downcase`         | tutto minuscolo                                |
| `.reverse`          | stringa capovolta                              |
| `.include?(sub)`    | `true` se `sub` è contenuta nella stringa      |
| `.count("aeiou")`   | conta le occorrenze dei caratteri elencati     |
| `.gsub(old, new)`   | sostituisce tutte le occorrenze di `old` con `new` |
| `.split(sep)`       | divide in array usando `sep` come separatore   |
| `.strip`            | rimuove spazi iniziali e finali                |
| `.chomp`            | rimuove il carattere di fine riga `\n`         |
| `.start_with?(s)`   | `true` se inizia con `s`                       |
| `.end_with?(s)`     | `true` se termina con `s`                      |

---

## Visitare una stringa

Per esaminare ogni carattere di una stringa si usa un ciclo. Con l'indice
si accede a ogni posizione; con `each_char` si itera direttamente sui
caratteri.

#### Esercizio 9
Copia il seguente codice nell'editor e fallo eseguire. Il programma
classifica ogni carattere come vocale o consonante.

{% highlight ruby %}
print "Inserisci una parola: "
parola = gets.chomp
vocali = "aeiouAEIOU"
indice = 0
while indice < parola.length
  lettera = parola[indice]
  if vocali.include?(lettera)
    puts "#{lettera} → vocale"
  else
    puts "#{lettera} → consonante"
  end
  indice = indice + 1
end
{% endhighlight %}

Tabella di tracciamento per la parola "ciao":

| indice | lettera | output      |
|:------:|:-------:|-------------|
| 0      | c       | consonante  |
| 1      | i       | vocale      |
| 2      | a       | vocale      |
| 3      | o       | vocale      |

#### Esercizio 10
Copia il seguente codice nell'editor e fallo eseguire. `each_char` è lo
stile idiomatico Ruby per visitare i caratteri: più leggibile del ciclo
con indice.

{% highlight ruby %}
print "Inserisci una parola: "
parola = gets.chomp
vocali = "aeiouAEIOU"
parola.each_char do |lettera|
  if vocali.include?(lettera)
    puts "#{lettera} → vocale"
  else
    puts "#{lettera} → consonante"
  end
end
{% endhighlight %}

---

## Esercizi

#### Esercizio 11
Scrivi un programma che lette due stringhe le scriva in ordine di lunghezza
(prima la più corta).

#### Esercizio 12
Scrivi un programma che letta una stringa la scriva tante volte quanti sono
i caratteri che la compongono.

#### Esercizio 13
Scrivi un programma che lette due stringhe le scriva in ordine alfabetico.

#### Esercizio 14
Scrivi un programma che lette tre stringhe le scriva in ordine alfabetico.

#### Esercizio 15
Scrivi un programma che visiti una stringa e conti quante vocali contiene.

#### Esercizio 16
Scrivi un programma che visiti una stringa e conti separatamente vocali
e consonanti.

#### Esercizio 17
Scrivi un programma che letta una stringa crei una nuova stringa
sostituendo tutte le `s` (maiuscole e minuscole) con `$` e tutte le `e`
(maiuscole e minuscole) con `€`.

#### Esercizio 18
Scrivi un programma che lette due stringhe di testo componga una nuova
stringa alternando i caratteri delle due stringhe iniziali.
Esempio: `"casa"` e `"rosa"` → `"craossaa"`.

#### Esercizio 19
Scrivi un programma che letto un numero intero calcoli la somma delle
cifre che lo compongono (tratta il numero come stringa).
Esempio: 124 → 7.

#### Esercizio 20
Scrivi un programma che applichi il **cifrario di Cesare** a una stringa
letta in input: ogni lettera viene spostata in avanti di `k` posizioni
nell'alfabeto (usa `.ord` e `.chr`). Il numero `k` viene letto in input.

#### Esercizio 21
Scrivi un programma che legga 3 numeri e generi il relativo istogramma
usando il carattere `*`.
Esempio: input 3, 5, 2 → output:
```
***
*****
**
```
