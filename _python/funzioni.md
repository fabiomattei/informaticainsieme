---
title: Funzioni
date: '2020-02-08T11:59:27+01:00'
author: Fabio Mattei
layout: page
---

Python è un linguaggio e in quanto tale è costituito da un vocabolario che comprente molte parole, tra queste ricordiamo input, print, if, while e for. Attraverso queste parole il programmatore comunica all’interprete, e quest’ultimo al computer, le operazioni da svolgere.

Il vocabolario di Python non è fisso ma estensibile possiamo cioè definire nuove parole con nuovi significati. Queste vengono chiamte funzioni e raggruppano al loro interno una sequenza di istruzioni ordinata (algoritmo) da eseguire quando vengono invocate.

#### La mia prima funzione 

Iniziamo con un esempio, definiamo una funzione hello che si limita a scrivere sul display alcune stringhe di testo:

{% highlight python %}
def hello():
    print('Ciao!')
    print('Ciao!!!')
    print('Ciao a te!')
{% endhighlight %}

L’istruzione def ci permette di definire una nuova funzione. Questa viene seguita dalla parola che vogliamo utilizzare come nome per la funzione, in questo caso hello. La parola hello viene seguita da una coppia di parentesi che si aprono e subito dopo si chiudono, vedremo in seguito qual’è il loro scopo.

Dato che, come abbiamo detto in precedenza, una funzione raggruppa al suo interno una sequenza di istruzioni da eseguire quando questa viene invocata, troviamo, al chiudersi delle parentesi, il simbolo : e sotto di questi il blocco di istruzioni da eseguire.

A questo punto l’interprete sa che quando incontra l’invocazione della funzione hello deve eseguire le istruzioni in essa contenute.

{% highlight python %}
print('Precede la chiamata a funzione')
hello()
print('Segue la chiamata a funzione')
{% endhighlight %}

Output:


{% highlight shell %}
Precede le chiamate a funzione Ciao!
Ciao!!!
Ciao a te!
Ciao!
Ciao!!!
Ciao a te!
Ciao!
Ciao!!!
Ciao a te!
Segue le chiamate a funzione
{% endhighlight %}

Una delle finalità principali delle funzioni è quella di raggruppare istru- zioni che vengono utilizzate molte volte. Se non fosse stata definita una funzione avreste dovuto copiare e incollare il codice al suo interno molte volte e lo script avrebbe avuto questo aspetto:

{% highlight python %}
print('Precede le chiamate a funzione')
print('Ciao!')
print('Ciao!!!')
print('Ciao a te!')
print('Ciao!')
print('Ciao!!!')
print('Ciao a te!')
print('Ciao!')
print('Ciao!!!')
print('Ciao a te!')
print('Segue le chiamate a funzione')
{% endhighlight %}

Definire funzioni aiuta ad essere più efficaci nella scrittura di codice dato che aiuta a tenere gli script più brevi.

## Parametri e argomenti 

Molte delle istruzoni Python viste in precedenza sono a loro volta delle funzioni. Per esempio la funzione print() è una di queste. Quando chiamate la funzione print(), le passate dei valori chiamati argomenti scrivendoli tra parentesi.

{% highlight python %}
print('Io sono un argomento')


{% endhighlight %}

Ad esempio in questo script viene invocata la funzione print() passan- dole, come argomento, la stringa di testo ’Io sono un argomento’.

Proviamo a definire la funzione hello in questo modo:

{% highlight python %}
def hello(nome):
  print('Ciao ' + nome)
{% endhighlight %}

La definizione della funzione hello in questo programma ha un para- metro chiamato nome. Un parametro è una variabile in cui viene memorizzato un argomento nel momento in cui viene chiamata la funzione. La prima volta che la funzione viene chiamata le viene passata la stringa Alice come argomento, l’esecuzione del programma entra nella fun- zione e la variabile nome viene impostata ad “Alice”. L’istruzione print va dunque a stampare la stringa Ciao Alice. L’argomento, memorizzato nel parametro, viene dimenticato al terminare dell’esecuzione della funzione. È un po’ come quello che succede alle variabili di un programma che vengono dimenticate una volta giunto a conclusione.

## I valori di ritorno 

{% highlight python %}
lunghezza = len('Ciao')
{% endhighlight %}

Nell’esempio in alto, si utilizza la funzione len, passandole come argo- mento la stringa ’Ciao’ al fine di far calcolare la lunghezza della stringa pas- sata. Il valore calcolato viene restituito al chiamante e da questi memorizzato nella variabile lunghezza.

Quando chiamate la funzione len e le passate un argomento come ’Ciao’ la funzione calcola come valore l’intero 4, la lunghezza della stringa. In generale il valore che una funzione calcola viene chiamato valore di ritorno. Quando si crea una funzione è possibile specificare quale sia il valore di ritorno attraverso l’istruzione return. Facciamo un esempio:

{% highlight python %}
def raddoppia(numero):
  return numero * 2
{% endhighlight %}

La funzione raddoppia accetta un parametro e restituisce il doppio dell’argomento che le viene passato.

Facciamo un secondo esempio:

{% highlight python %}
def pariodispari(numero):
  if numero%2:
    return 'Pari'
  else:
    return 'Dispari'
{% endhighlight %}

La funzione pariodispari accetta un parametro e restituisce la stringa ’Pari’ se l’argomento passato è pari e la stringa ’Dispari’ in caso contrario. È possibile inserire due o più istruzioni di return in una funzione. Duran- te l’esecuzione del programma (a runtime) una di questa prima poi verrà eseguita. Quando questo accade il computer esce dalla funzione e torna al programma chiamante portando con se il valore di ritorno.

Notate che quando l’interprete incontra l’istruzione return esce dall’am- bito della funzione e torna nel programma da cui questa era stata chiamata. Nessuna istruzione all’interno della funzione viene eseguita dopo il return.

{% highlight python %}
primonumero = pariodispari(4)   # var primonumero = 'Pari' 
secondonumero = pariodispari(7) # var secondonumero = 'Dispari'

{% endhighlight %}

## Parametri facoltativi 

È possibile definire delle funzioni con dei parametri facoltativi, parametri cioè per cui viene specificato un argomento di default che viene utilizzato a meno che non specificato diversamente dal chiamante.

Facciamo un esempio

{% highlight python %}
def moltiplica(fattore1, fattore2 = 2):
    return fattore1 * fattore2
{% endhighlight %}

Il parametro fattore2 viene inizializzato a 2 se non specificato diversamente dal chiamante.

{% highlight python %}
prodotto = moltiplica(3) 
print(prodotto) # stampa il valore 6 
prodotto = moltiplica(3, 3) 
print(prodotto) # stampa il valore 9
{% endhighlight %}

Notate che i parametri facoltativi vanno sempre specificati dopo aver specificato tutti i parametri ordinari dunque se si definisce una funzione come nell’esempio seguente l’interprete restituisce un messaggio di errore di sintassi.

{% highlight python %}
def moltiplica(fattore1 = 2, fattore2): 
    return fattore1 * fattore2
{% endhighlight %}

## Ambito locale e ambito globale 

Parametri e variabili definiti all’atto della definizione di una funzione hanno un ambito (o raggio d’azione, in inglese scope) locale alla funzione stessa. Le variabili assegnate all’esterno di tutte le funzioni hanno invece un ambito globale. Una variabile non può essere contemporaneamente locale e globale.

Pensate ad un ambito come ad una sorta di contenitore di variabili. Quando un ambito viene distrutto tutti i valori conservati nelle variabili di quell’ambito vengono dimenticati. Esiste un solo ambito globale e viene creato quando inizia il programma. Esistono tanti ambiti locali quante sono le funzioni che definiamo.

Un ambito locale viene creato ogni volta che si chiama una funzione. Qualsiasi variabile che riceva assegnazione in quella funzione esiste solo nel- l’ambito locale. Quando la funzione ritorna al chiamante l’ambito locale viene distutto e quelle variabili vengono dimenticate. Alla seguente chia- mata della stessa funzione le variabili locali non ricorderanno i valori che vi erano memorizzati nella precedente chiamata. Valgono le seguenti proprietà:

- istruzioni nell’ambito globale non possono usare variabili che appartengono ad un ambito locale;
- istruzioni nell’ambito locale possono liberamente accedere a variabili definite in ambito globale;
- istruzioni contenute all’interno di un ambito locale non possono accedere a variabili appartenenti ad un diverso ambito locale;
- è possibile utilizzare lo stesso nome per variabili diverse se si trovano in ambiti diversi.

È considerata buona norma non modificare il valore di una variabile globale all’interno di una funzione (effetto collaterale o in inglese side effect)

## Le varabili locali non possono essere utilizzate nell’ambito globale 

{% highlight python %}
def pollaio(): 
    uova = 32765
pollaio()
print(uova) # NameError: name 'uova' is not defined
{% endhighlight %}

Come potete notare la variabile uova appartiene all’ambito delle funzione pollaio. La funzione viene invocata, ma una volta terminata la variabile locale uova viene distrutta, non può dunque essere utilizzata dal flusso di programma principale.

## Gli ambiti locali non possono usare variabili di altri ambiti locali 

{% highlight python %}
def pollaio():
    uova = 32765
    print(uova) # corretto

def allevamento():
    pecore = 1234567
    print(pecore) # corretto
    print(uova)   # NameError:'uova' not defined
{% endhighlight %}

Come potete vedere la funzione allevamento tenta di accedere alla variabile uova che appartiene all’ambito della funzione pollaio. Questo non è legale.

## Le variabili globali possono essere lette da un ambito locale 

{% highlight python %}
uova = 1234567 
def pollaio():
    print(uova) # corretto 
pollaio()
{% endhighlight %}

La funzione pollaio riesce ad accedere alla variabile uova definita in ambito globale.

## Variabili locali e globali con lo stesso nome 

{% highlight python %}
uova = 7 # variabile globale
def pollaio():
    uova = 32765
    print(uova) # corretto, stampa 32765
def allevamento(): 
    uova = 1234567
    print(uova) # corretto, stampa 1234567
print(uova) # corretto, stampa 7
{% endhighlight %}

La variabile uova viene definita sia nell’ambito globale, che nell’ambito della funzione pollaio che nell’ambito della funzione allevamento. Ciascun ambito riesce però ad accedere alla variabile ivi definita.

## Collaborare attraverso le funzioni 

Quando un gruppo di sviluppatori lavora ad un software un aspetto mol- to delicato è quello della divisione dei compiti e del lavoro. Un approccio che spesso viene utilizzato è quello di strutturare il software identificando- ne le varie caratteristiche quindi organizzare queste ultime all’interno delle varie funzioni. Se per esempio la classe 3C volesse creare un software per la gestione della biblioteca della scuola, potrebbe dividere il lavoro in varie sezioni (interfaccia utente, salvataggio dati, regole per l’utilizzo del servi- zio, servizi di notifiche, servizi di calendario). A questo punto il gruppo stabilisce un linguaggio comune per la comunicazione esplicitando la firma di tutte le funzioni. Questo consiste nell’esplicitare il nome delle funzioni che andranno a costituire il software, i parametri che queste accettano e i valori che queste riportano al chiamante. Tutto ciò va fatto prima dell’implementazione delle funzioni stesse. In questo modo gli sviluppatori possono lavorare in modo indipendente sapendo esattamente come il loro lavoro andrà a integrarsi nel resto del sistema software che si sta creando.

## Inizia scrivendo un test 

Supponiamo di voler scrivere una funzione che deve fare la trasformazione di gradi Celsius in gradi Fahrenheit. Noi tutti sappiamo che: Tf = Tc \* 9 / 5 + 32.

Dato che dobbiamo sviluppare questa importante funzione matematica diventa tedioso iniziare a scrivere il codice e poi inserire l’input per controllare che la funzione si comporti come atteso. Ci serviamo dunque dell’istruzione assert.

Assert accetta due argomenti: il primo è una proposizione booleana che può essere vera o falsa (True o False) e nel caso questa non lo sia stampa il messaggio che segue la virgola.

{% highlight python %}
def trasfCelsInFahr(temp):
    return (temp*9/5)+32
assert trasfCelsInFahr(0) == 32.0, "Errore per 0" 
assert trasfCelsInFahr(20) == 68.0, "Errore per 20" 
assert trasfCelsInFahr(30) == 86.0, "Errore per 30"
{% endhighlight %}

Quando vado a lanciare il programma questo manderà in esecuzione tutti i test e nel caso uno di questi non funzioni, cioè se la mia implementazione non è corretta, visualizza un messaggio di errore. Questo espediente mi consente di correggere l’implementazione della funzione dato che è molto rapido eseguire i test per vedere se tutto funziona come atteso.

Un test rappresenta anche una forma di contratto tra programmato- ri. Quando due programmatori collaborano su di una applicazione possono accordarsi sulla firma delle funzioni e su alcuni esempio che scrivono sotto forma di test. In questo modo sarà chiaro per entrambi quando la funzione è ben implementata.

Ultimo vantaggio un test mi permette di fare refactoring del codice. Questo significa che posso lavorare sull’implementazione della funzione per migliorarla dal punto di visto delle prestazioni per esempio, essendo sicuro di non introdurre bugs nel codice dato che di volta in volta controllo che tutti i test diano esito positivo.

## Scrivere la documentazione per ciascuna funzione 

È molto importante lasciare traccia su quanto viene calcolato da una funzione. Ciò che appare scontato durante il processo di scrittura di una funzione potrebbe non esserlo più quando la si va a riutilizzare un anno più tardi. Bisogna poi imparare ad essere buoni cittadini del mondo digitale e pensare che altri potrebbero utilizzare le funzioni che scriviamo e dunque è buona norma descriverne in maniera puntuale il funzionamento.

{% highlight python %}
def trasfCelsInFahr(temp):
    """ This function trasforms the temperature provided in parameter temp from Fahrenheit scale to Celsius scale
    temp: float
    return: float
    """
    return (Temperature*9/5)+32
{% endhighlight %}

## Organizza il tuo codice in moduli 

Quando si scrive il codice è bene scriverlo in modo che questo sia facile da tenere sotto controllo con i test e che questo sia riutilizzabile in una o più applicazioni. Per questo è importante organizzare il codice in moduli. Immaginiamo di lavorare su di un progetto di fisica. Saggiamente decidiamo di creare una cartella che conterrà tutti i file del nostro progetto e, senza sforzare troppo la nosrta fantasia la chiameremo progettodifisica.

- progettodifisica/
- progettodifisica/principale.py
- progettodifisica/grandezze/conversioni.py

La cartella progettodifisica contiene un file principale.py il quale contiene la sezione principale del codice, quello cioè che mando in esecuzione quando ho necessità di utilizzare il software.

{% highlight python %}
# principale.py

# importa il modulo conversioni.py
from grandezze.conversioni import *

def main():
    temp = float(input('Temp Celsius: ')) 
    fahr = trasfCelsInFahr(temp) 
    print('Temp. Fahrenheit = '+str(fahr))

# se lo script viene mandato in 
# esecuzione quanto contenuto in 
# questo if viene eseguito
if __name__ == '__main__':
    main()
{% endhighlight %}

La funzione main contiene il flusso principale del programma.

L’istruzione from dà indicazioni all’interprete di caricare la libreria che abbiamo appena definito. È buona norma metterla in alto, all’inizio del file. La sua sintassi è:

from nomercartella.nomefile import nomefunzione

Qualora la variabile di sistema \_\_name\_\_ corrisponda alla stringa \_\_main\_\_ si innesca la chiamata alla funzione main(). Questa riga consente al nostro codice di capire se sta venendo eseguito come script a se stante, o se è invece stato richiamato come modulo.

{% highlight python %}
# grandezze/conversioni.py

def trasfCelsInFahr(Temp):
  return Temp*9/5+32

def trasfFahrInCels(Temp):
  pass
  
# se lo script viene mandato in 
# esecuzione quanto contenuto in 
# questo if viene eseguito
if __name__ == '__main__':
    assert trasfCelsInFahr(0) == 32.0, "Err. per 0" 
    assert trasfCelsInFahr(20) == 68.0, "Err. per 20" 
    assert trasfCelsInFahr(30) == 86.0, "Err. per 30" 
    print("Tutto ok!")
{% endhighlight %}

In seguito troviamo un file di libreria in cui abbiamo precedentemente scritto l’implementazione delle funzioni che mi occorrono per fare conversioni tra grandezze. Questo file è contenuto nella cartella grandezze e si chiama conversioni.py. Questo file contiene due funzioni la prima trasforma i gradi Celsius in Fahrenheit, la seconda fa il contrario trasforma i gradi Fahrenheit in gradi Celsius. Notare che nella parte finale dentro l’if ci sono i test che si occupano di assicurarci la buona qualità della libreria. Se mando in run il file conversioni.py e visualizzo la scritta “tutto ok” vuol dire che la libreria funziona come ci si aspetta.

## Esercizi 

#### Esercizio 1:

Scrivi una funzione saluta che prenda la stringa nome come parametro e restituisca al chiamate la stringa composta da ’Ciao ’ + nome.

#### Esercizio 2:

Scrivi una funzione calcolamaggiore che prenda due numeri come parametro (num1 e num2) e restituisca al chiamante il più grande tra i due.

#### Esercizio 3:

Scrivi una funzione calcolamaggiore che prenda tre numeri come parametro (num1, num2 e num3) e restituisca al chiamante il più grande tra i tre.

#### Esercizio 4:

Scrivi una funzione a cui viene passato un carattere come parametro, e che restituisca al chiamante la stringa ’vocale’ se il carattere è una vocale o la stringa ’consonante’ in caso contrario.

#### Esercizio 5:

Indovina cosa fa la funzione mistero

{% highlight python %}
def mistero(parametro):
    output = 0
    for x in parametro:
        if x == '0':
            output = output * 10 + 0
        elif x == '1':
            output = output * 10 + 1
        elif x == '2':
            output = output * 10 + 2
        elif x == '3':
            output = output * 10 + 3
        elif x == '4':
            output = output * 10 + 4
        elif x == '5':
            output = output * 10 + 5
        elif x == '6':
            output = output * 10 + 6
        elif x == '7':
            output = output * 10 + 7
        elif x == '8':
            output = output * 10 + 8
        elif x == '9':
            output = output * 10 + 9
    return output
{% endhighlight %}

#### Esercizio 5:

Riorganizza il seguente codice Python in funzioni. Bisogna soddividere l’algoritmo in algoritmi piú piccoli e semplici da capire in modo da rendere il codice più leggibile.

{% highlight python %}
print('La mia calcolatrice') 
finito = False
while finito == False:
    op = input('Operatore (+ - * /): ') 
    num1 = input('Primo numero: ')
    num2 = input('Secondo numero: ')
    if op == '+':
        risultato = num1 + num2 
    elif op == '-':
        risultato = num1 - num2 
    elif op == '*':
        risultato = num1 * num2 
    elif op == '/':
        risultato = num1 / num2 
    else:
        risultato = 'Operatore sconosciuto' 
    risp = input('Finito? (S, N): ')
    if risp == 'S':
        finito = True
print('Grazie! Alla prossima volta')
{% endhighlight %}

#### Esercizio 6:

Implementa una funzione che preso come parametro un numero intero restituisca al chiamante il corrispondente numero di Fibonacci.

#### Esercizio 7:

Scrivi con approccio Test Drive Development una funzione che calcoli il massimo comune divisotre tra due numeri.

#### Esercizio 8:

Scrivi con approccio Test Drive Development una funzione che calcoli il minimo comune multiplo tra due numeri.

#### Esercizio 9:

Scrivere una funzione con approccio TDD che calcoli la distanza tra due punti sul piano cartesiano.
firma: distanza(ax, ay, bx, by)

#### Esercizio 7:

Cosa fa il seguente script?

{% highlight python %}
biciclette = 123
def scrivi_biciclette(): 
    print(biciclette)
scrivi_biciclette()
{% endhighlight %}

#### Esercizio 8:

Cosa fa il seguente script?

{% highlight python %}
biciclette = 123
def scrivi_biciclette(): 
    biciclette = 321 
    print(biciclette)
scrivi_biciclette()
{% endhighlight %}

#### Esercizio 9:

Crea una funzione **divisibile\_per** che prenda come parametro due numeri e che restituisca True se il primo numero è divisibile per il secondo e False in caso contrario

#### Esercizio 10:

Scrivi una funzione in python chiamata calcola stipendio che prende come parametro il numero di ore lavorate in una settimana e la paga oraria.

def calcolastipendio(ore, paga):

La funzione se il numero di ore è minore o uguale a 40 “ritorna” il prodotto tra il primo numero e il secondo. Se è maggiore: per le prima 40 ora ritorna il primo numero per il secondo per le ore di straordinario rotorna il primo numero per il secondo maggiorato del 50%.

Esempi:  
calcolastipendio(20, 20) =&gt; 400   
calcolastipendio(40, 20) =&gt; 800   
calcolastipendio(50, 20) =&gt; 1100

#### Esercizio 11:

Utilizziamo le funzioni per calcolare delle spese di viaggio: Definisci una funzione chiamata **costo\_hotel** che prende come parametro il numero delle notti. L’hotel costa 140 per notte. La funzione deve calcolare il costo totale dell’hotel. Definisci una funziona chiamata **costo\_aereo** questa prende come parametro il nome della città in cui si vola e ritorna a seconda della destinazione: “Charlotte”: 183 “Tampa”: 220 “Pittsburgh”: 222 “Los Angeles”: 475. Definisci una funziona **noleggio\_macchina** che prenda come parametro il numero di giorni. Ogni giorno di noleggio macchina costa 40. Se si noleggia la macchina per più di 7 giorni si ottiene uno sconto di 50. Se si noleggia la macchina per più di 10 giorni si ottiene uno sconto di 10 al giorno. In fine crea una funzione **costo\_viaggio** che prenda come parametro una stringa rappresentante la destinazione e il numero di giorni di viaggio e chiamando le funzioni prima definite calcoli il costo totale del viaggio.

#### Esercizio 12:

Crea una funzione **tipo\_stringa** che prenda come parametro una stringa di testo e restituisca:   
* la stringa “solo lettere” se il parametro è costitutito completamente da lettere   
* la stringa “solo numeri” se il parametro è costitutito completamente da numeri   
* la stringa “mista” se il parametro è costitutito completamente sia da lettere sia da numeri

Esempi:  
tipo\_stringa(“1322132”) =&gt; “solo numeri”  
print(tipo\_stringa(“acbac”)) =&gt; “solo lettere”  
print(tipo\_stringa(“132acbac12”)) =&gt; “mista”

#### Esercizio 13:

Scrivi una funzione **esprimi\_giudizio(voto)** che preso come parametro il voto di uno studente restituisca una stringa di testo con giudizio secondo la seguente tabella:

| 1, 2, 3, 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|
| Gravemente insufficiente | Insufficiente | Sufficiente | Discreto | Buono | Distinto | Ottimo |

#### Esercizio 14

Scrivi una funzione python somma\_numeri che prenda come parametri due numeri interi a e b e calcoli la somma di tutti i numeri compresi tra a e b con a e b compresi   
Esempio:   
somma\_numeri(4, 6) restituisce 15   
somma\_numeri(1, 4) restituisce 10

#### Esercizio 15

Scrivi una funzione python somma\_pari che prenda come parametri due numeri interi a e b e calcoli la somma di tutti i numeri pari compresi tra a e b con a e b compresi   
Esempio:   
somma\_numeri(4, 6) restituisce 10   
somma\_numeri(1, 5) restituisce 6

#### Esercizio 16

Scrivi una funzione python is\_prime che prenda come parametro un numero interi a e restituisca True se a è primo e False in caso contrario Esempio:   
is\_prime(11) restituisce True   
is\_prime(4) restituisce False

#### Esercizio 17

Scrivi una funzione celsiusToFahrenheit che accetti come parametro una temperatura in gradi Celsius e restituisca la corrispondente temperatura inn gradi Fahrenheit. Scrivere poi una funzione FahrenheitToCelsius che faccia l'operazione opposta. Scrivi in fine il main che facendo uso delle funzioni scrivi le scale di conversione di temperatura per i gradi Celsius che vanno da -20°C a 100°C a passo 5 e per i gradi Fahrenheit che vanno da -5 a 205 a passo 10.

#### Esercizio 18

Crea una libreria di funzioni turtle che disegnino figure geometriche. Le firme delle funzioni da scrivere sono: def quadrato(lato): def rettangono(base, altezza): def cerchio(raggio) def esagono(lato) Crea in fine una funzione main() che richiami le quattro funzioni appena descritte con dei valori dati in input dall’utente.
