---
title: 'Ruby: il ciclo until'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Il ciclo until

Ruby è uno dei pochi linguaggi a disporre della parola chiave `until`.
`until` esegue il blocco **finché la condizione è falsa**, terminando non
appena diventa vera. È l'equivalente di `while not`.

La sintassi è la seguente:

{% highlight ruby %}
until <condizione>
  <istruzione 1>
  <istruzione 2>
end
{% endhighlight %}

Come `while`, anche `until` ha il **controllo in testa**: la condizione viene
valutata prima di ogni iterazione. Se è già vera all'inizio, il blocco non
viene mai eseguito.

La differenza con `while` è solo di leggibilità: si sceglie `until` quando
la condizione si esprime più naturalmente come "fino a quando non succede
qualcosa" piuttosto che "mentre qualcosa è vero".

| Costruzione         | Significato                    |
|---------------------|--------------------------------|
| `while cont < 5`    | esegui mentre cont è minore di 5 |
| `until cont >= 5`   | esegui finché cont non raggiunge 5 |

Le due forme sono equivalenti. Quella più leggibile dipende dal contesto.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
cont = 0
until cont >= 5
  cont = cont + 1
  puts cont
end
{% endhighlight %}

Possiamo tracciare il valore di `cont` a ogni iterazione:

| Iterazione | cont (prima) | cont (dopo) | Condizione cont >= 5 |
|:----------:|:------------:|:-----------:|:--------------------:|
| 1          | 0            | 1           | falsa → continua     |
| 2          | 1            | 2           | falsa → continua     |
| 3          | 2            | 3           | falsa → continua     |
| 4          | 3            | 4           | falsa → continua     |
| 5          | 4            | 5           | falsa → continua     |
| —          | 5            | —           | **vera** → uscita    |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire. Osserva come
`until` rende la condizione di uscita più diretta rispetto a `while`.

{% highlight ruby %}
acc = 0
cont = 1
until cont > 10
  acc = acc + cont
  cont = cont + 1
end
puts acc
{% endhighlight %}

Il ciclo somma i numeri da 1 a 10. Scrivere `until cont > 10` si legge
naturalmente: "continua finché il contatore non supera 10".

| cont | acc prima | acc dopo |
|:----:|:---------:|:--------:|
| 1    | 0         | 1        |
| 2    | 1         | 3        |
| 3    | 3         | 6        |
| …    | …         | …        |
| 10   | 45        | 55       |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire. Il ciclo continua
finché l'utente non inserisce la parola corretta: con `until` la condizione
di uscita si legge in modo molto naturale.

{% highlight ruby %}
print "Digita 'stop' per uscire: "
risposta = gets.chomp
until risposta == "stop"
  puts "Hai scritto: #{risposta}"
  print "Digita 'stop' per uscire: "
  risposta = gets.chomp
end
puts "Uscita dal ciclo."
{% endhighlight %}

Se avessimo usato `while`, avremmo dovuto scrivere `while risposta != "stop"`,
che richiede di ragionare sulla negazione. `until risposta == "stop"` esprime
direttamente la condizione che ci interessa: "aspetta finché non arriva stop".

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire. Questo programma
continua a leggere numeri e li somma finché l'accumulatore non raggiunge
o supera una soglia stabilita.

{% highlight ruby %}
soglia = 100
acc = 0
cont = 0
until acc >= soglia
  print "Inserisci un numero: "
  n = gets.chomp.to_i
  acc = acc + n
  cont = cont + 1
end
puts "Hai inserito #{cont} numeri. Totale: #{acc}"
{% endhighlight %}

## Il ciclo do-while con until (begin … end until)

Come `while`, anche `until` ha la variante `begin … end until` che esegue
il corpo **almeno una volta** prima di valutare la condizione. È utile
quando vogliamo eseguire un'azione e poi decidere se ripeterla.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
begin
  print "Inserisci un numero compreso tra 1 e 10: "
  n = gets.chomp.to_i
end until n >= 1 && n <= 10
puts "Hai inserito: #{n}"
{% endhighlight %}

La condizione `n >= 1 && n <= 10` viene verificata **dopo** la lettura.
Se il numero non è nell'intervallo, si richiede un nuovo valore. Il programma
non può proseguire finché non riceve un input valido.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire. Il menu si ripresenta
finché l'utente non sceglie l'opzione di uscita.

{% highlight ruby %}
scelta = 0
begin
  puts "--- Menu ---"
  puts "1. Saluta"
  puts "2. Conta fino a 5"
  puts "3. Esci"
  print "Scelta: "
  scelta = gets.chomp.to_i
  if scelta == 1
    puts "Ciao!"
  elsif scelta == 2
    for i in 1..5
      print "#{i} "
    end
    puts
  end
end until scelta == 3
puts "Arrivederci."
{% endhighlight %}

Questo schema — mostra menu, esegui l'azione, ripeti — è un caso tipico
di `begin … end until`: il menu deve apparire almeno una volta.

---

## Esercizi

#### Esercizio 7
Scrivi un programma che letto un numero positivo N determini il massimo
intero K tale che la somma dei primi K interi sia minore o uguale a N.
Ad esempio, se N=20 allora K=5, infatti 1+2+3+4+5=15 mentre 1+2+3+4+5+6=21.

#### Esercizio 8
Scrivi un programma che letti due numeri interi calcoli il massimo comune
divisore (MCD) usando l'algoritmo di Euclide.

#### Esercizio 9
Scrivi un programma che letto un numero intero N trovi il più piccolo
multiplo di 7 che sia maggiore di N.

#### Esercizio 10
Scrivi un programma che letto un numero intero positivo N conti quante
cifre lo compongono (senza convertirlo in stringa).

#### Esercizio 11
Scrivi un programma che simuli un conto alla rovescia: letto un numero N,
stampa i numeri da N fino a 1, poi scrive "Via!".

#### Esercizio 12
Scrivi un programma che legga numeri interi uno alla volta e si fermi
non appena trova un numero negativo. Al termine stampa quanti numeri
positivi sono stati inseriti e la loro somma.
