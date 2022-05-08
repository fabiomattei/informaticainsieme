---
id: 297
title: 'La programmazione ad oggetti'
date: '2020-02-09T07:36:40+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=297'
---

#### Introduzione

Nella programmazione procedurale distinguiamo tra dati, e funzioni. I primi costituiscono l’insieme delle informazioni su cui operiamo, le seconde costituiscono l’insieme degli algoritmi che operano sui dati. Nella programmazione tradizionale queste sono due entità distinte.

Il kernel linux è custotuito da oltre 25 milioni di righe di codice scritte da 19000 programmatori, il progetto Libreoffice è custotuito da circa 10 milioni di righe di codice scritte da pidi 1800 programmatori.

Quando il codice diventa molto ampio si aprono problematiche come:

- collisioni di nomi: funzioni o variabili che vengono definite in sezioni diverse di programma, da persone diverse, con lo stesso nome. Quando il computer si trova di fronte a queste non sa cosa fare;
- side effects: una variabile viene utilizzata da un programmatore in un modo e da un altro programmatore in un altro, questo crea comportamenti inaspettati da parte del software.

 Nella programmazione ad oggetti si cerca di ovviare a queste problematiche creando dei pacchetti di informazioni e di funzioni che operano su queste. I primi si chiamano attributi, i secondi si chiamano metodi.

#### Sintassi 

La classe più semplice che possiamo definire in Python è:

{% highlight python %}
class Test:
    pass

{% endhighlight %}

</div>Come possiamo vedere dall’esempio, per definire una classe basta usare la parola chiave class, seguita dal nome che vogliamo dare alla classe (in questo caso Test), seguita dai due punti (:), seguita infine da un blocco di codice indentato (in questo caso c’è solo il pass) che è il codice appartenente alla classe stessa.

Proviamo ora a definire una classe Cerchio.

{% highlight python %}
class Cerchio: 

    pigreco = 3.14
    
    def __init__(self, raggio):
        """ Inizializza la classe 
        parametro raggio int o float
        """ 
        self.raggio = raggio
        
    def calcola_area(self):
        """ Calcola l'area del cerchio 
        return float
        """ 
        return self.raggio ** 2 * self.pigreco
        
    def calcola_circonferenza(self):
        """ Calcola la circonferenza del cerchio 
        return float
        """ 
        return 2 * self.pigreco * self.raggio
{% endhighlight %}

</div>La classe Cerchio ha un **attributo di classe** chiamato *pigreco*. È un attributo di classe perchè ha lo stesso valore per tutte le istanze della classe.

Questo vuol dire che posso definire molti cerchi, ognuno con un raggio diverso da quello degli altri, ma per tutti il valore di π sarà sempre lo stesso.

La classe ha anche un attributo di istanza chiamato raggio. Questi sarà impostato di volta in volta in modo differente per ciascun cerchio appartenente alla classe.

La classe Cerchio ha due metodi: calcola\_raggio e calcola\_circonferenza. Questi si occupano di operare sulle informazioni contenute nella classe per implementare gli algoritmi necessari.

Nota che mentre gli attributi di classe sono targhette per identificare le informazioni e dunque sono descritti da sostantivi, i metodi sono targhette per identificare algoritmi e sono dunque descritti da verbi.

#### Classi 

La classe è un costrutto che ci permette di definire la base per creare oggetti. All’interno di una classe si definisce la strutture delle informazioni su cui questa lavorerà e gli algoritmi che opereranno sulle informazioni stesse. **Le classi sono costrutti astratti non si riferiscono a nessun oggetto specifico**. Ad esempio posso definire la classe *Alunno* che al suo interno conterrà le stringhe nome e cognome senza entrare nella specificità dell’alunno che si chiama Mario Rossi.

{% highlight python %}
class Alunno:
    
    def __init__(self, nome, cognome):
    """ Costruttore
    parametro nome string parametro cognome string 
    """ 
    self.nome = nome 
    self.cognome = cognome
    
    def saluta(self):
    """ Scrive una stringa di saluto
    return string 
    """
    return "Ciao, mi chiamo " + self.nome + " " + self.cognome
{% endhighlight %}

</div>#### Istanze 

Le istanze sono oggetti creati a partire da una classe. Sono le materializzazione della classe stessa. Al fine di creare una istanza si utilizza il nome con cui è stata definita la classe e tra parentesi si forniscono gli argomenti esplicitati in via di definizione del costruttore \_\_init\_\_.

{% highlight python %}
mario = Alunno("Mario", "Rossi") 
print(mario.saluta())
rita = Alunno("Rita", "Morelli") 
print(rita.saluta())
cerchio5 = Cerchio(5.5) 
print(cerchio5.calcola_circonferenza())
{% endhighlight %}

</div>#### Attributi 

Le informazioni contenute in una classe vengono memorizzate attraverso gli attributi. Ad esempio la classe Alunno contiene gli attributi nome e cognome. Questi sono accessibili liberamente dai metodi della classe ma è buona abitudine renderli inaccessibili dall’esterno, questo perchè un accesso diretto da parte del consumer sugli attributi va venire meno il principio di incapsulamento. Esistono due tipi di attributi:

- attributi di classe
- attributi di istanza

Gli attributi di istanza sono informazioni che sono definite istanza per istanza, ad esempio il nome e il cognome della classe Alunno i secondi sono informazioni comuni a tutte le istanze, per esempio il valore di π per la classe Cerchio. È bene che ciascun attributo sia descritto da un sostativo.

Gli attributi di classe sono valori legati alla classe, che sono comuni e accessibili da tutte le istanze. Per dichiarare attributi di classe, esistono due modi: usando classe.attributo = valore o usando attributo = valore nel corpo della dichiarazione di una classe.

#### Metodi 

I metodi sono gli algoritmi che operano con le informazioni contenute negli attributi. Dato che i metodi sono sostanzialmente nomi di algoritmi che “fanno cose” è bene che questi siano dei verbi. Per ragioni implementative ciascun metodo definito contiene il parametro fittizio self. Questo parametro va sempre esplicitato per ciascun metodo, ma va ignorato quando il metodo viene chiamato.

- le definizioni dei metodi si trovano indentate all’interno della classe;
- la sintassi usata per definire i metodi è uguale a quella usata per definire le funzioni;
- i metodi devono definire un parametro aggiuntivo che per convenzione è chiamato self;
- per chiamare un metodo basta usare la sintassi istanza.metodo().

#### la parola chiave self 

Come si nota nell’esempio precedente, la differenza più importante tra la definizione di metodi e funzioni, è la presenza del self. self è un argomento che si riferisce all’istanza, e anche se i metodi devono dichiararlo esplicitamente, non è necessario passarlo esplicitamente.

Dato che self si riferisce all’istanza, possiamo usarlo per accedere ad altri attributi e metodi definiti all’interno dello classe semplicemente facendo self.attribute o self.metodo(). Inizializzare istanze Le classi supportano anche diversi metodi “speciali” che sono identificati dalla presenza di due *underscore* prima e dopo del nome. Questi metodi non vengono chiamati diretta- mente facendo inst.\_\_metodo\_\_, ma vengono in genere chiamati automaticamente in situazioni particolari Uno di questi metodi speciali è \_\_init\_\_, chiamato automaticamente ogni volta che un’istanza viene creata.

#### Incapsulamento 

Progettare un software attraverso l’approccio Object oriented consiste nel trovare informazioni logicamente correlate e inserirle all’interno di una classe. Queste informazioni lavoreranno a stretto contatto le une con le altre attraverso gli algoritmi (metodi) che fanno parte della classe stessa.

Quando si parla di incapsulamento si intendono due aspetti della progettazione del software:

- mettere insieme informazioni e metodi logicamente connessi in una classe;
- nascondere i dettagli implementativi al di fuori della classe stessa.

#### Ereditarietà 

La particolarità della programmazione ad oggetti consiste nel creare gerarchie di classi che condividono metodi ed attributi al fine di permettere al programmatore di definire di volta in volta non l’intero ammontare di informazioni e algoritmi ma soltanto ciò che varia.

Ad esempio potrei generalizzare la classe Alunno definita in precedenza nella sua classe padre Persona.

{% highlight python %}
class Persona:
    def __init__(self, nome, cognome):
        """ Costruttore 
        parametro nome string 
        parametro cognome string 
        """
        self.nome = nome 
        self.cognome = cognome
        
    def saluta(self):
        """ Scrive una stringa di saluto 
        return string
        """
    return "Ciao, mi chiamo " + self.nome + " " + self.cognome
{% endhighlight %}

</div>A questo punto definisco la classe Studente in maniera più semplice, ereditando i metodi \_\_init\_\_ e saluta dalla classe padre. Per lo studente definisco il solo metodo lavora.

{% highlight python %}
class Studente(Persona):
    def lavora(self):
        print("Sto studiando")
        
class Professore(Persona):
    def lavora(self):
        print("Sto spiegando")
    
{% endhighlight %}

</div>#### Polimorfismo 

{% highlight python %}
scuola = []
scuola.append(Alunno("Mario", "Rossi")) 
scuola.append(Alunno("Rita", "Morelli")) 
scuola.append(Professore("Antonino", "Anile"))
for persona in scuola: 
    persona.lavora()
{% endhighlight %}

</div>Il polimorfismo entra in atto quando ho oggetti di tipo differente (che appartengono a classi diverse) che hanno la stessa interfaccia. Le istanze di una sottoclasse possono essere utilizzate al posto delle istanze della superclasse. L’overriding dei metodi o delle proprietà permette che gli oggetti appartenenti alle sottoclassi di una stessa classe rispondano diversamente agli stessi utilizzi.

Per esempio nell’esempio in alto abbiamo tre oggetti, i primi due appartenenti alla classe Alunno e il terzo appartenente alla classe Professore. Dato che tutti questi oggetti definiscono il metodo lavora è possibile scrivere un costrutto come quello contenuto nel ciclo for.

#### Composizione

Negli esempi che abbiamo visto finora gli attributi  
delle classi erano variabili di tipo primitivo è però possibile definire come attributi dei riferimenti ad oggetti di un’altra classe.  
In questo modo abbiamo oggetti composti da altri  
oggetti.

Creiamo una classe scuola che contenga al suo interno una lista di studenti, una lista di professori, una lista di persone che compongono il personale ATA ed un preside.

{% highlight python %}
class Scuola:
    def __init__(self):
        self.preside = None
        self.alunni = []
        self.professori = []
        self.personale_ata = []
    
    def aggiungi_studente(self, studente):
        self.studenti.append(studente)
        
    def definisci_preside(self, preside):
        self.preside = preside
        
    def aggiungi_professore(self, professore):
        self.professori.append(professore)
    
    def aggiungi_ata(self, ata):
        self.professori.personale_ata(ata)
        
    def conta_studenti(self):
        return len(self.studenti)
        
    def conta_professori(self):
        return len(self.professori)
        
{% endhighlight %}

</div>La classe appena definita contiene anche alcuni metodi che lavorano sugli oggetti contenuti. I metodi *conta\_studenti* e *conta\_professori* reestituiscono il numero di studenti e di professori che sono contenuti nelle rispettive liste.

A questo punto è possibile utilizzare la classe appena definita come segue:

{% highlight python %}
patiniliberatore = Scuola()         
        
patiniliberatore.aggiungi_studente(Alunno("Mario", "Rossi")) 
patiniliberatore.aggiungi_studente(Alunno("Rita", "Morelli")) 
patiniliberatore.aggiungi_professore(Professore("Antonino", "Anile"))

print("Nel Patini studiano", patiniliberatore.conta_studenti(), "studenti")
{% endhighlight %}

</div>Notiamo che alla riga 1 creiamo una nuova istanza della classe scuola. Dalla riga 3 alla 5 popoliamo la scuola con due studenti ed un professore. In fine alla riga 7 utilizziamo il metodo *conta\_studenti* per contare il numero di studenti nella scuola.

#### Inizia scrivendo un test 

Supponiamo di voler scrivere una classe che si occupi di trasformare i gradi Celsius in gradi Fahrenheit. Noi tutti sappiamo che: *Tf = Tc \* 9 / 5 + 32*

Dato che dobbiamo sviluppare questa importante funzione matematica diventa tedioso iniziare a scrivere il codice e poi inserire l’input per controllare che la funzione si comporti come atteso. Ci serviamo dunque dell’istruzione **assert**.

Assert accetta due argomenti: il primo è una proposizione booleana che può essere vera o falsa (True o False) e nel caso questa non lo sia stampa il messaggio che segue la virgola.

{% highlight python %}
class Conversioni:
    def trasfCelsInFahr(Temp):
        return Temp*9/5+32
    def trasfFahrInCels(Temp):
        pass
    
# se lo script viene mandato in # esecuzione quanto contenuto in # questo if viene eseguito
if __name__ == '__main__':
    conv = Conversioni()
    assert conv.trasfCelsInFahr(0) == 32.0, "Err. per 0" 
    assert conv.trasfCelsInFahr(20) == 68.0, "Err. per 20" 
    assert conv.trasfCelsInFahr(30) == 86.0, "Err. per 30" 
    print("Tutto ok!")
{% endhighlight %}

</div>Quando vado a lanciare il file questo manderà in esecuzione tutti i test e nel caso uno di questi non funzioni, cioè se la mia implementazione non è

corretta, visualizzerà un messaggio di errore. Questo espediente mi consente di correggere l’implementazione della classe e dei suoi metodi dato che è molto rapido eseguire i test per vedere se tutto funziona come atteso.

Un test rappresenta anche una forma di contratto tra programmatori. Quando due programmatori collaborano su di una applicazione possono accordarsi sulla firma delle classi e dei loro metodi e su alcuni esempi che scrivono sotto forma di test. In questo modo sarà chiaro per entrambi quando il codice è ben implementata.

Ultimo vantaggio: un test mi permette di fare refactoring del codice. Questo significa che posso lavorare sull’implementazione della classe per migliorarla dal punto di visto delle prestazioni per esempio, essendo sicuro di non introdurre bugs nel codice dato che di volta in volta che faccio modifiche controllerò che tutti i test diano esito positivo.

#### Scrivere la documentazione per ciascuna classe 

È molto importante lasciare traccia su quanto viene implementato. Ciò che appare scontato durante il processo di scrittura potrebbe non esserlo più quando si va a rileggere quel codice un anno più tardi. Bisogna poi imparare ad essere buoni cittadini del mondo digitale e pensare che altri potrebbero utilizzare le funzioni che scriviamo e dunque è buona norma descriverne in maniera puntuale il funzionamento.

{% highlight python %}
class Conversioni:
    """ Questa classe implementa alcune trasformazioni 
    tra diverse scale di temperatura 
    """
    
    def trasfCelsInFahr(temp):
        """ Questo metodo trasforma la temperatura 
        data in gradi Celsius
        in temperatura in gradi Fahrenheit
        temp: float
        return: float 
        """
        return temp*9/5+32
        
    def trasfFahrInCels(Temp):
        pass
    
{% endhighlight %}

</div>#### Organizza il tuo codice in moduli 

Quando si scrive il codice è bene scriverlo in modo che questo sia facile da tenere sotto controllo con i test e che questo sia riutilizzabile in una o più applicazioni. Per questo è importante organizzare il codice in moduli. Immaginiamo di lavorare su di un progetto di fisica. Saggiamente decidiamo di creare una cartella che conterrà tutti i file del nostro progetto e, senza sforzare troppo la nostra fantasia la chiameremo progettodifisica.

- progettodifisica/
- progettodifisica/principale.py
- progettodifisica/grandezze/conversioni.py

La cartella progettodifisica contiene un file principale.py il quale contiene la sezione principale del codice, quello cioè che mando in esecuzione quando ho necessità di utilizzare il software.

{% highlight python %}
# file: principale.py

# importa il modulo conversioni.py
from grandezze.conversioni import *

def main():
    temp = float(input('Temp Celsius: ')) 
    conv = new Conversioni()
    fahr = conv.trasfCelsInFahr(temp) 
    print('Temp. Fahrenheit = '+str(fahr))
    # se lo script viene mandato in 
    # esecuzione quanto contenuto in 
    # questo if viene eseguito
    
if __name__ == '__main__':
    main()
    
{% endhighlight %}

</div>La funzione main contiene il flusso principale del programma.

L’istruzione **from** dà indicazioni all’interprete di caricare la libreria che abbiamo appena definito. È buona norma metterla in alto, all’inizio del file. La sua sintassi è:

*from nomercartella.nomefile import nomeclasse*

Qualora la variabile di sistema \_\_name\_\_ corrisponda alla stringa \_\_main\_\_ si innesca la chiamata alla funzione main(). Questa riga consente al nostro codice di capire se sta venendo eseguito come script a se stante, o se è invece stato richiamato come modulo.

{% highlight python %}
# file: grandezze/conversioni.py

class Conversioni:
    
    def trasfCelsInFahr(self, Temp):
        return Temp*9/5+32
        
    def trasfFahrInCels(self, Temp):
        pass

# se lo script viene mandato in 
# esecuzione quanto contenuto in 
# questo if viene eseguito
if __name__ == '__main__':
    conv = Conversioni()
    assert conv.trasfCelsInFahr(0) == 32.0, "Err. per 0" 
    assert conv.trasfCelsInFahr(20) == 68.0, "Err. per 20" 
    assert conv.trasfCelsInFahr(30) == 86.0, "Err. per 30" 
    print("Tutto ok!")
    
{% endhighlight %}

</div>In seguito troviamo un file di libreria in cui abbiamo precedentemente scritto l’implementazione delle funzioni che mi occorrono per fare conversioni tra grandezze. Questo file è contenuto nella cartella grandezze e si chiama conversioni.py. Questo file contiene due funzioni la prima trasforma i gradi Celsius in Fahrenheit, la seconda fa il contrario trasforma i gradi Fahrenheit in gradi Celsius. Notare che nella parte finale dentro l’if ci sono i test che si occupano di assicurarci la buona qualità della libreria. Se mando in run il file conversioni.py e visualizzo la scritta “tutto ok” vuol dire che la libreria funziona come ci si aspetta.

L’esempio che abbiamo appena visto ci dà idea su come gestire il codice in moduli che posso riutilizzare su molte applicazioni.

### Esercizi 

**Esercizio 1:** Completa la struttura descritta all’interno del capitolo aggiungendo altre 5 classi di persone che ruotano nell’ambito del mondo della scuola.

**Esercizio 2:** Crea la struttura di classi necessaria per descrivere gli animali all’interno di uno zoo. Parti dalla classe base Animali e poi definisci le sottoclassi Mammiferi Rettili Uccelli e poi continua creando una classe per ciascun tipo di Animale: Orso, Vipera ecc. fino a 10 animali. Le classi avranno un attributo di classe per il nome scientifico, un attributo di istanza per il nome de singolo e avranno un metodo per presentarsi ed un per fare il verso (Bau bau, Miao, Zzzzz ecc.).

**Esercizio 3:** Immagina di dover fare una applicazione per pagare gli stipendi in una azienda. Il direttore guadagna 100000 euro/anno, il vi- cedirettore 70000, una manager di medio livello 50000, gli impiegati 35000. Le ore di straordinario vengono retribuite il 20% più del normale. Chi fa il part-time guadagna il 60% del suo stipendio lordo. Crea la struttura di classi e i metodi per calcolare gli stipendi del personale in basso.