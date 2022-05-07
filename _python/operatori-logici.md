---
id: 115
title: 'Operatori logici'
date: '2020-02-04T15:07:35+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=115'
---

E’ possibile combinare tra loro le espressioni logiche attraverso gli operatori logici

<figure class="wp-block-table">| A | B | A and B |
|---|---|---|
| True | True | True |
| True | False | False |
| False | True | False |
| False | False | False |

</figure><figure class="wp-block-table">| A | B | A or B |
|---|---|---|
| True | True | True |
| True | False | True |
| False | True | True |
| False | False | False |

</figure><figure class="wp-block-table">| A | not A |
|---|---|
| True | False |
| False | True |

</figure>**Esempio:**  
Scrivere in programma Python che letta la temperatura dell’acqua scriva lo stato in cui questa si trova: solida, liquida, gassosa

{% highlight python %}
temperatura = int( input("Scrivi la temperatura dell'acqua: ")
if temperatura <= 0:
   print("Stato solido")
if temperatura >0 and temperatura <=100:
   print("Stato liquido")
if temperatura > 100:
   print("Stato Gassoso")
{% endhighlight %}

</div>Notiamo che la stringa di testo “Stato liquido” viene scritta se, e soltanto se, la temperatura è sia maggiore di 0 sia minore o uguale a 100. Devono essere verificate entrambe le espressioni affinché l’espressione totale sia verificata.

Esercizio 16: Al fine di calcolare le imposte da versare al fisco lo stato italiano predispone 5 scaglioni. La tassazione viene calcolata limitatamente alla porzione di reddito che ricade in ciascuno scaglione IRPEF.

<figure class="wp-block-table">| Scaglioni reddito Irpef | Aliquota |
|---|---|
| da 0 a **15.000** euro | 23% |
| da **15.000,01** a **28.000** euro | 27% |
| Da **28.000,01** a **55.000** euro | 38% |
| da **55.000,01** a **75.000** euro | 41% |
| oltre **75.000** euro | 43% |

</figure>Esercizio 17: scrivi un programma che letto un numero scriva se questo è positivo, negativo o nullo.

Esercizio 18: scrivere un programma che letto un numero a virgola mobile scriva se questo è positivo, negativo o nullo e aggiunga il messaggio small se il valore assoluto del numero è minore di uno oppure large se è superiore ad un milione.

Esercizio 19: Scrivere un programma che letto un numero intero e visualizzi il numero delle sue cifre verificando prima se il numero è &gt;= a 10, poi se il numero è &gt;= 100 e così via fino a 10 miliardi. Se il numero è negativo moltiplicarlo prima per -1.

Esercizio 20: Scrivere un programma che letti 3 numeri interi visualizzi ”tutti uguali” se questi sono tutti uguali e “tutti differenti” se sono tutti differenti. Scriva “non uguali non differenti” in caso contrario.

Esercizio 21: Scrivere un programma che legga 3 numeri interi e scriva “in ordine” se sono ordinati in senso crescente o decrescente e “non in ordine” in caso cotrario  
Es: 1, 2, 5 → in ordine; 6, 4, 2 → in ordine; 7, 2, 9 → non in ordine

Esercizio 22: Scrivere un programma che letta una temperatura (intera) e l’unità di misura (Celsius o Fahrenheit) indichi se l’acqua a quella temperatura si trovi allo stato solido, liquido o gassoso.

Esercizio 23: Scrivere un programma che acquisisca dall’utente la descrizione di una carta da gioco usando la notazione abbreviata:  
A: Asso  
1..10: Valore della carta  
J: fante (Jack)   
Q: regina (Queen)  
K: re (King)  
D: Quadri (diamonds)  
H: cuori (hearts)  
S: picche (Spades)  
C: Clubs (fiori)  
Il programma deve poi visualizzare la descrizione completa della carta  
Es: QS: Queen of spades

Esercizio 24:  
Scrivere un programma che letti due istanti di tempo espressi in ora e minuti determini quale istante preceda l’altro e li scriva in ordine.  
Es: Input Tempo1: HH:12 MM:30 Tempo 2: HH:07 MM:45 &gt; Output 07:45 → 12:30

Esercizio 25:  
Scrivere un programma Python che chieda all’utente la sua data di nascita e visualizzi il suo segno zodiacale

Esercizio 26:  
Scrivere un programma Python che chieda all’utente una data e visualizzi la stagione cui la data appartiene

Esercizio 27:  
Un anno viene detto bisestile se divisibile per 4. Gli anni come 1996 xsono bisestili. Ma gli anni divisibili per 100 non lo sono, ad esempio il 1900 non lo è. Eccezione dell’eccezione gli anni divisibili per 400 lo sono, ad esempio l’anno 2000.  
Scrivere un programma che legga un anno e scriva se questo è bisestile oppure no.