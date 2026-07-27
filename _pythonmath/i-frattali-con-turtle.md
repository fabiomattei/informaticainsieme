---
id: 138
title: 'I frattali con turtle'
date: '2020-02-04T15:57:56+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=138'
---

Un **frattale** è una figura geometrica che si ripete in modo simile a se stessa a ogni livello di ingrandimento: se si osserva un dettaglio del frattale con una "lente" sempre più potente, si continuano a ritrovare forme che ricordano la figura di partenza. Questa proprietà si chiama **autosomiglianza** (*self-similarity*) e, a differenza delle figure della geometria classica (cerchi, quadrati, triangoli), permette a un frattale di avere un livello di dettaglio potenzialmente infinito pur essendo generato da una regola molto semplice, spesso ripetuta ricorsivamente.

### Chi ha inventato i frattali

Il termine "frattale" fu coniato nel 1975 dal matematico polacco-francese **Benoit Mandelbrot**, che lo derivò dal latino *fractus* (spezzato, frammentato). Mandelbrot lavorava alla IBM e si accorse che moltissime forme naturali — le coste frastagliate, i fiocchi di neve, i rami degli alberi, le felci, i vasi sanguigni, le nuvole — non potevano essere descritte con le forme regolari della geometria euclidea, ma seguivano invece uno schema di ripetizione a diverse scale. Nel 1980, grazie alla potenza di calcolo dei computer, Mandelbrot riuscì anche a visualizzare per la prima volta il celebre **insieme di Mandelbrot**, forse l'immagine più conosciuta legata ai frattali.

In realtà, prima di Mandelbrot, alcuni matematici avevano già costruito figure con queste proprietà, anche se all'epoca venivano considerate poco più che curiosità matematiche prive di applicazioni pratiche, "mostri" geometrici che sembravano violare le regole dell'analisi classica:

- il matematico svedese **Helge von Koch** descrisse nel 1904 la curva che oggi porta il suo nome, alla base del fiocco di neve che vedremo in questa pagina;
- il matematico polacco **Wacław Sierpiński** descrisse nel 1915 il triangolo che porta il suo nome;
- già nel 1883 il matematico tedesco **Georg Cantor** aveva costruito un insieme (il "insieme di Cantor") con proprietà di autosomiglianza.

Fu però Mandelbrot a unificare queste figure sotto un'unica teoria — la **geometria frattale** — e a mostrare che non erano semplici stranezze matematiche, ma un modo più accurato di descrivere la complessità delle forme che troviamo in natura.

In questa pagina costruiamo due frattali classici usando la libreria **turtle** di Python: il fiocco di neve di Koch e un albero ricorsivo. Entrambi si basano sulla stessa idea di fondo: una funzione che richiama se stessa (**ricorsione**) per disegnare, a ogni livello, una versione più piccola della figura di partenza.

## Il fiocco di neve di Koch

L'algoritmo si basa sulla **curva di Koch**: si parte da un segmento e lo si divide in tre parti uguali; la parte centrale viene sostituita con i due lati di un triangolo equilatero costruito su di essa, ottenendo così una "punta" al posto del terzo centrale. Applicando la stessa regola, ricorsivamente, a ciascuno dei quattro nuovi segmenti ottenuti, si ottiene una linea sempre più frastagliata. Se si applica la curva di Koch a ciascuno dei tre lati di un triangolo equilatero, il risultato è il **fiocco di neve di Koch**.

Nella funzione `koch(n, d)`:

- `n` rappresenta il **livello di ricorsione** rimasto: quando vale 1 siamo al **caso base** e la funzione si limita a disegnare un segmento dritto lungo `d`;
- quando `n` è maggiore di 1 siamo nel **caso ricorsivo**: il segmento di lunghezza `d` viene diviso in tre parti (`d3 = d/3`) e la funzione richiama se stessa quattro volte con un livello di ricorsione inferiore (`n-1`), intervallando le chiamate con le rotazioni (`left(60)`, `right(120)`, `left(60)`) che disegnano la "punta" del triangolino al centro del segmento.

Il ciclo `for` finale ripete questo procedimento tre volte, ruotando di 120 gradi tra una ripetizione e l'altra, in modo da disegnare i tre lati del triangolo di partenza. La variabile `RICORSIONE` controlla quante volte la regola di Koch viene applicata (più è alta, più il contorno risulta dettagliato e frastagliato), mentre `DISTANZA` controlla la lunghezza del lato di partenza del triangolo.

<figure class="wp-block-image size-large"><img src="/images/pythonmath/i-frattali-con-turtle/frattalistella.png" alt=""></figure>

{% highlight python %}
import turtle
turtle.hideturtle()
turtle.pencolor('red')
turtle.speed(0)
turtle.penup()
turtle.setx(-260)
turtle.sety(-150)
turtle.left(60)
turtle.pendown()
def koch(n,d):
   if(n == 1):
      turtle.forward(d)
   elif(n > 1):
      d3=d/3
      koch(n-1,d3); turtle.left(60)
      koch(n-1,d3); turtle.right(120)
      koch(n-1,d3); turtle.left(60)
      koch(n-1,d3)
RICORSIONE=4
DISTANZA=550
for i in range(1,RICORSIONE+1):
   turtle.pensize(i)
   koch(i,DISTANZA); turtle.right(120)
   koch(i,DISTANZA); turtle.right(120)
   koch(i,DISTANZA); turtle.right(120)
turtle.done()
{% endhighlight %}

## L'albero frattale

Il secondo esempio riprende un'idea molto usata in natura: un ramo che si divide in due rami più corti, ciascuno dei quali si divide a sua volta in altri due rami, e così via. Anche in questo caso la figura si costruisce con una funzione ricorsiva, `albero(n, d)`:

- `n` è il **livello di ricorsione** rimasto: quando raggiunge 0 siamo al **caso base** e la funzione non disegna altro, terminando quel ramo;
- finché `n` è maggiore di 0 siamo nel **caso ricorsivo**: la tartaruga avanza di `d` (disegnando il ramo), poi la funzione richiama se stessa due volte — una girando a sinistra di 45 gradi e una a destra di 45 gradi — per disegnare i due rami figli, ciascuno più corto del precedente (`d2 = d/RATIO`) e con un livello di ricorsione inferiore (`n-1`).

Il dettaglio più delicato dell'algoritmo è come la tartaruga torna indietro dopo aver disegnato ciascun ramo figlio: le rotazioni `left(45)`, poi (dopo la prima chiamata ricorsiva) `right(90)`, poi (dopo la seconda) `left(45)`, riportano la tartaruga a puntare nella direzione originale del ramo padre, mentre `backward(d)` la riporta esattamente al punto da cui era partita. In questo modo, senza bisogno di salvare e ripristinare esplicitamente posizione e orientamento della tartaruga, la funzione lascia le cose esattamente come le ha trovate prima di procedere con il ramo successivo.

I tre parametri `RICORSIONE`, `DISTANZA` e `RATIO` controllano rispettivamente quante volte l'albero si ramifica, la lunghezza del tronco di partenza e di quanto si accorcia ogni ramo figlio rispetto al padre: provando a modificarli si ottengono alberi più o meno frondosi e più o meno "aperti".

<figure class="wp-block-image size-large"><img src="/images/pythonmath/i-frattali-con-turtle/frattalialbero.png" alt=""></figure>

{% highlight python %}
import turtle
#--------------------------
turtle.shape('turtle')
turtle.pencolor('red')
turtle.pensize(3)
turtle.speed(0)
turtle.penup()
turtle.sety(-280)
turtle.pendown()
turtle.left(90)
#--------------------------
def albero(n,d):
    if(n > 0):
        d2=d/RATIO
        turtle.forward(d)
        turtle.left(45)
        albero(n-1,d2)
        turtle.right(90)
        albero(n-1,d2)
        turtle.left(45)
        turtle.backward(d)
#--------------------------
RICORSIONE=9
DISTANZA=250
RATIO=1.65
albero(RICORSIONE,DISTANZA)
turtle.done()
{% endhighlight %}