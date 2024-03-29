---
title: 'La programmazione ad oggetti'
date: '2020-02-09T07:36:40+01:00'
author: Fabio Mattei
layout: page
---

## Introduzione

Nella programmazione procedurale distinguiamo tra dati, e funzioni. I primi costituiscono l’insieme delle informazioni su cui operiamo, le seconde costituiscono l’insieme degli algoritmi che operano sui dati. Nella programmazione tradizionale queste sono due entità distinte.

Il [kernel linux](https://github.com/torvalds/linux) è custituito da oltre 25 milioni di righe di codice scritte da 19000 programmatori, il progetto [Libreoffice](https://github.com/LibreOffice/core) è custituito da circa 10 milioni di righe di codice scritte da più di 1800 programmatori.

Quando il codice diventa molto ampio si aprono problematiche come:

- collisioni di nomi: funzioni o variabili che vengono definite in sezioni diverse di programma, da persone diverse, con lo stesso nome. Quando il computer si trova di fronte a queste non sa cosa fare;
- side effects: una variabile viene utilizzata da un programmatore in un modo e da un altro programmatore in un altro, questo crea comportamenti inaspettati da parte del software.

 Nella programmazione ad oggetti si cerca di ovviare a queste problematiche creando dei pacchetti di informazioni e di funzioni che operano su queste. I primi si chiamano attributi, i secondi si chiamano metodi.

## Sintassi 

La classe più semplice che possiamo definire in Python è:

{% highlight python %}
class Test:
    pass

{% endhighlight %}

Come possiamo vedere dall’esempio, per definire una classe basta usare la parola chiave class, seguita dal nome che vogliamo dare alla classe (in questo caso Test), seguita dai due punti (:), seguita infine da un blocco di codice indentato (in questo caso c’è solo il pass) che è il codice appartenente alla classe stessa.

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

La classe Cerchio ha un **attributo di classe** chiamato *pigreco*. È un attributo di classe perchè ha lo stesso valore per tutte le istanze della classe.

Questo vuol dire che posso definire molti cerchi, ognuno con un raggio diverso da quello degli altri, ma per tutti il valore di π sarà sempre lo stesso.

La classe ha anche un attributo di istanza chiamato raggio. Questi sarà impostato di volta in volta in modo differente per ciascun cerchio appartenente alla classe.

La classe Cerchio ha due metodi: calcola\_raggio e calcola\_circonferenza. Questi si occupano di operare sulle informazioni contenute nella classe per implementare gli algoritmi necessari.

Nota che mentre gli attributi di classe sono targhette per identificare le informazioni e dunque sono descritti da sostantivi, i metodi sono targhette per identificare algoritmi e sono dunque descritti da verbi.

## Classi 

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

## Istanze 

Le istanze sono oggetti creati a partire da una classe. Sono le materializzazione della classe stessa. Al fine di creare una istanza si utilizza il nome con cui è stata definita la classe e tra parentesi si forniscono gli argomenti esplicitati in via di definizione del costruttore \_\_init\_\_.

{% highlight python %}
mario = Alunno("Mario", "Rossi") 
print(mario.saluta())
rita = Alunno("Rita", "Morelli") 
print(rita.saluta())
cerchio5 = Cerchio(5.5) 
print(cerchio5.calcola_circonferenza())
{% endhighlight %}

## Attributi 

Le informazioni contenute in una classe vengono memorizzate attraverso gli attributi. Ad esempio la classe Alunno contiene gli attributi nome e cognome. Questi sono accessibili liberamente dai metodi della classe ma è buona abitudine renderli inaccessibili dall’esterno, questo perchè un accesso diretto da parte del consumer sugli attributi va venire meno il principio di incapsulamento. Esistono due tipi di attributi:

- attributi di classe
- attributi di istanza

Gli attributi di istanza sono informazioni che sono definite istanza per istanza, ad esempio il nome e il cognome della classe Alunno i secondi sono informazioni comuni a tutte le istanze, per esempio il valore di π per la classe Cerchio. È bene che ciascun attributo sia descritto da un sostativo.

Gli attributi di classe sono valori legati alla classe, che sono comuni e accessibili da tutte le istanze. Per dichiarare attributi di classe, esistono due modi: usando classe.attributo = valore o usando attributo = valore nel corpo della dichiarazione di una classe.

## Metodi 

I metodi sono gli algoritmi che operano con le informazioni contenute negli attributi. Dato che i metodi sono sostanzialmente nomi di algoritmi che “fanno cose” è bene che questi siano dei verbi. Per ragioni implementative ciascun metodo definito contiene il parametro fittizio self. Questo parametro va sempre esplicitato per ciascun metodo, ma va ignorato quando il metodo viene chiamato.

- le definizioni dei metodi si trovano indentate all’interno della classe;
- la sintassi usata per definire i metodi è uguale a quella usata per definire le funzioni;
- i metodi devono definire un parametro aggiuntivo che per convenzione è chiamato self;
- per chiamare un metodo basta usare la sintassi istanza.metodo().

## la parola chiave self 

Come si nota nell’esempio precedente, la differenza più importante tra la definizione di metodi e funzioni, è la presenza del self. self è un argomento che si riferisce all’istanza, e anche se i metodi devono dichiararlo esplicitamente, non è necessario passarlo esplicitamente.

Dato che self si riferisce all’istanza, possiamo usarlo per accedere ad altri attributi e metodi definiti all’interno dello classe semplicemente facendo self.attribute o self.metodo(). Inizializzare istanze Le classi supportano anche diversi metodi “speciali” che sono identificati dalla presenza di due *underscore* prima e dopo del nome. Questi metodi non vengono chiamati diretta- mente facendo inst.\_\_metodo\_\_, ma vengono in genere chiamati automaticamente in situazioni particolari Uno di questi metodi speciali è \_\_init\_\_, chiamato automaticamente ogni volta che un’istanza viene creata.

## Incapsulamento 

Progettare un software attraverso l’approccio Object oriented consiste nel trovare informazioni logicamente correlate e inserirle all’interno di una classe. Queste informazioni lavoreranno a stretto contatto le une con le altre attraverso gli algoritmi (metodi) che fanno parte della classe stessa.

Quando si parla di incapsulamento si intendono due aspetti della progettazione del software:

- mettere insieme informazioni e metodi logicamente connessi in una classe;
- nascondere i dettagli implementativi al di fuori della classe stessa.

In effetti l'incapsulamento è proprio legato al concetto di **impacchettare**. Si impacchettano all'interno di una classe i dati e le azioni che sono riconducibili ad un singolo componente.

Un altro modo di guardare all'incapsulamento, che abbiamo già accennato, è quello di pensare a suddividere un'applicazione in piccole parti (le classi) che raggruppano al loro interno alcune funzionalità legate tra loro.

Pensiamo ad esempio all'automobile. Una automobile possiende delle proprietà come: nome, modello, azienda produttruce, tipo di motore, cilindrata ecc. L'automobile inoltre può fare della azioni come: apri le porte, spostati, fai scattare l'allarme ecc. Queste proprietà e questi metodi, dato cono sono **correlati**, sono insieme all'interno della classe Automobile. L'effetto è che tutto è raccolto quindi quando si dovrà fare manutenzione e modificare la classe, si saprà dove andare ad intervenire. 

Manutenere un software è normalmente più difficoltoso che costruirlo. E' bene che attributi e dati siano vicini per evitare il problema della **duplicazione**. La duplicazione dei metodi e degli attributi è molto pericolosa. E' possibile che nel tempo ci si dimentichi di due metodi **gemelli** e se ne aggiorni uno solo inserendo immancabilmente bug nel codice.

## Ereditarietà 

La programmazione ad oggetti permette di creare gerarchie di classi che condividono metodi ed attributi al fine di permettere al programmatore di definire di volta in volta non l’intero ammontare di informazioni e algoritmi ma soltanto ciò che varia.

Supponiamo di aver definito due classi Docente ed Alunno che definiscono il metodo **saluta** e il metodo **lavora**. Il metodo saluta è analogo per entrambe le classi mentre il metodo lavora è diverso. Entrambe le classi contengono gli attributi nome e cognome ma la classe Docente contiene l'attributo materia mentre la classe Alunno contiene l'attributo classe.

![Ereditarietà](/images/python/oggetti/ereditarieta1.png){:class="aside-image"}

{% highlight python %}
class Docente:
    def __init__(self, nome, cognome, materia):
        """ Costruttore 
        parametro nome string 
        parametro cognome string 
        parametro materia string 
        """
        self.nome = nome 
        self.cognome = cognome
        self.materia = materia
        
    def saluta(self):
        """ Scrive una stringa di saluto 
        return string
        """
        return "Ciao, mi chiamo " + self.nome + " " + self.cognome
	
    def lavora(self):
        """ Scrive una stringa per spiegare cosa fa 
        return string
        """
        return "Sto insegnando " + self.materia

class Alunno:
    def __init__(self, nome, cognome, classe):
        """ Costruttore 
        parametro nome string 
        parametro cognome string 
        parametro classe string 
        """
        self.nome = nome 
        self.cognome = cognome
        self.classe = classe
        
    def saluta(self):
        """ Scrive una stringa di saluto 
        return string
        """
        return "Ciao, mi chiamo " + self.nome + " " + self.cognome
	
    def lavora(self):
        """ Scrive una stringa per spiegare cosa fa 
        return string
        """
        return "Sto frequentando la classe " + self.classe
{% endhighlight %}

L'ereditarietà mi permette di astrarre in una classe genitore le informazioni e i metodi comuni in modo da rendere il codice molto più compatto. 

Definiamo dunque la classe Persona che contiene gli attributi **nome** e **cognome** e contiene il metodo **saluta**. Sono stati scelti questi attributi e questo metodo perché comuni sia alla classe Docente che alla classe Alunno.

Quando vado a definire la classe Docente indico il fatto che questa eredita da Persona mettendo il nome della classe Persona tra parentesi di fianco al nome della classe Docente. A questo punto la classe Docente erediterà da Persona attributi e metodi e le sue implementazioni potranno utilizzare questi attributi e questi metodi ereditati come se fossero parte integrande della classe Docente. Mi comporto analogamente con la classe Alunno.

Notiamo che il costruttore della classe docente è un po' diverso dal solito. Questo avviene perché vado a richiamare al suo interno il costruttore della classe padre in modo da non implementare due volte l'assegnazione degli attributi nome e cognome.

Notiamo come il codice diventa più snello dato che non ho bisogno di definire due volte gli stessi metodi e gli stessi attributi riducendo la ridondanza del codice.

![Ereditarietà](/images/python/oggetti/ereditarieta2.png){:class="aside-image"}

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
	
class Docente(Persona):
    def __init__(self, nome, cognome, materia):
        """ Costruttore 
        parametro nome string 
        parametro cognome string 
        parametro materia string 
        """
        super().__init__(nome, cognome)
        self.materia = materia
		
    def lavora(self):
        """ Scrive una stringa per spiegare cosa fa 
        return string
        """
        return "Sto insegnando " + self.materia
		
class Alunno(Persona):
    def __init__(self, nome, cognome, classe):
        """ Costruttore 
        parametro nome string 
        parametro cognome string 
        parametro classe string 
        """
        super().__init__(nome, cognome)
        self.classe = classe
		
    def lavora(self):
        """ Scrive una stringa per spiegare cosa fa 
        return string
        """
        return "Sto frequentando la classe " + self.classe
{% endhighlight %}

Sulla destra vediamo i diagrammi UML delle classi. Questi diagrammi servono a rendere visibile la gerarchia delle classi e l'interfaccia di ciascuna classe. Ogni classe viene identificata da un rettangolo che si divide in tre sezioni. Nella prima sezione si mette il nome della classe, nella seconda i suoi attributi e nella terza i suoi metodi. L'ereditarietà si rappresenta con una freccia che dalla  classe figlia va verso la classe.

A questo punto posso definire il main in questo modo:

{% highlight python %}
mario = Alunno("Mario", "Rossi", "4C")
rosa = Alunno("Rosa", "Verdi", "3C")
armando = Docente("Armando", "Bianchi", "Filosofia")

print(mario.saluta())
print(mario.lavora())

print(rosa.saluta())
print(rosa.lavora())

print(armando.saluta())
print(armando.lavora())
{% endhighlight %}




## Polimorfismo 

Si parla di polimorfismo quando ho oggetti di tipo differente (appartengono a classi diverse) che hanno la stessa interfaccia.

Spieghiamo meglio il concetto: l'interfaccia di un oggetto (che ricordiamo è l'istanza di una classe) è rappresentata dai metodi che sono definiti nella sua classe. Quindi se due classi diverse, implementano soltanto metodi aventi lo stesso nome, con gli stessi parametri, allora le classi hanno la stessa interfaccia.

I metodi che vengono ridefiniti in una sottoclasse sono detti **polimorfi**, in quanto lo stesso metodo si comporta diversamente a seconda della classe a cui appartiene l'oggetto dal quale è invocato. 

L'iterfaccia di una classe definisce un **contratto** generale che sottoclassi diverse possono soddisfare in modi diversi.

### Esempio: geometria 

{% highlight python %}
class Triangolo:
    def __init__(self, base, altezza):
        """Costruttore della classe Triangolo"""	
        self.base = base
        self.altezza = altezza
    
    def calcola_area(self):
        """Metodo che restituisce larea del triangolo"""	
        return self.base * self.altezza / 2
{% endhighlight %}


{% highlight python %}
class Rettangolo:
    def __init__(self, base, altezza):
        """Costruttore della classe Rettangolo"""	
        self.base = base
        self.altezza = altezza
    
    def calcola_area(self):
        """Metodo che restituisce larea del triangolo"""	
        return self.base * self.altezza
{% endhighlight %}

Notiamo che le classi appena definite implementano metodi con lo stesso nome e con gli stessi parametri, dunque hanno la stessa interfaccia.

A questo punto nel **main** posso scrivere:


{% highlight python %}
t1 = Triangolo(2, 4)   # creo una istanza di Triangolo
t2 = Triangolo(2, 6)   # creo una seconda istanza di Triangolo
r1 = Rettangolo(2, 3)  # creo una seconda istanza di Rettangolo

print(t1.calcola_area())       # invoco il metodo calcola_area() appartenente all'istanza t1 della classe Triangolo
print(t2.calcola_area())       # invoco il metodo calcola_area() appartenente all'istanza t2 della classe Triangolo
print(r1.calcola_area())       # invoco il metodo calcola_area() appartenente all'istanza r1 della classe Rettangolo
{% endhighlight %}

### Esempio: la scuola

{% highlight python %}
scuola = []
scuola.append(Alunno("Mario", "Rossi")) 
scuola.append(Alunno("Rita", "Morelli")) 
scuola.append(Professore("Antonino", "Anile"))
for persona in scuola: 
    persona.lavora()
{% endhighlight %}

Le istanze di una sottoclasse possono essere utilizzate al posto delle istanze della superclasse. L’overriding dei metodi o delle proprietà permette che gli oggetti appartenenti alle sottoclassi di una stessa classe rispondano diversamente agli stessi utilizzi.

Per esempio nell’esempio in alto abbiamo tre oggetti, i primi due appartenenti alla classe Alunno e il terzo appartenente alla classe Professore. Dato che tutti questi oggetti definiscono il metodo lavora è possibile scrivere un costrutto come quello contenuto nel ciclo for.


## Composizione

Negli esempi che abbiamo visto finora gli attributi delle classi erano variabili di tipo primitivo è però possibile definire come attributi dei riferimenti ad oggetti di un’altra classe. In questo modo abbiamo oggetti composti da altri oggetti.

### Esempio: l'appartamento

![Appartamento](/images/python/oggetti/appartamento.png){:class="aside-image"}

Facciamo un esempio. Immaginiamo di voler calcolare la superficie di un appartamento. L'Appartamento, visibile in piantina è composto da due camere da letto, un bagno una cucina ed un salotto. Il tutto è collegato da un corridoio centrale.

Se ci pensiamo un attimo notiamo che tutti questi ambienti, sono schematizzabili come rettangoli. Possiamo dunque definire una classe in questo modo:

{% highlight python %}
class Stanza:
    def __init__(self, nome, lunghezza, larghezza):
        """Costruttore della classe Stanza"""	
        self.nome = nome
        self.lunghezza = lunghezza
        self.larghezza = larghezza
    
    def calcola_superficie(self):
        """Metodo che restituisce la superficie della stanza"""	
        return self.lunghezza * self.larghezza
{% endhighlight %}

La nostra classe **Stanza** possiede tre attributi: il nome, la lunghezze e la larghezza. Una stanza è inoltre in grado di **calcolare la sua superficie** attraverso un metodo che restituisce il prodotto dell'attributo larghezza moltiplicato per l'attributo lunghezza.

A questo punto possiamo implementare l'appartamento come un contenitore di stanze:

{% highlight python %}
class Appartamento:
    def __init__(self, indirizzo):
        """Costruttore della classe Appartamento"""	
        self.indirizzo = indirizzo
        self.stanze = []
    
    def aggiungi_stanza(self, stanza):
        """Questo metodo aggiunge la stanza passata come parametro
        alla lista di stanze contenute nell'appartamento"""
        self.stanze.append(stanza)

    def calcola_superficie(self):
        """Calcola la superficie dell'intero appartamento
        sommando tra loro le superfici delle singole stanze"""	
        superficie = 0
        for stanza in self.stanze:
            superficie = superficie + stanza.calcola_superficie()
        return superficie
{% endhighlight %}

A questo punto possiamo implementare il **main** in questo modo:

{% highlight python %}
appartamentomio = Appartamento("via Mazzini, 22") # creo una istanza di appartamento
camera_grande = Stanza("Camera grande", 5, 5)     # creo una istanza di stanza per la camera grande
camera_piccola = Stanza("Camera piccola", 4, 5)   # creo una istanza di stanza per la camera piccola
bagno = Stanza("Bagno", 4, 2)                     # creo una istanza di stanza per il bagno
cucina = Stanza("Cucina", 4, 2)                   # creo una istanza di stanza per la cucina
salotto = Stanza("Salotto", 4, 2)                 # creo una istanza di stanza per il salotto
appartamentomio.aggiungi_stanza(camera_grande)    # aggiungo la camera grande al mio appartamento
appartamentomio.aggiungi_stanza(camera_piccola)
appartamentomio.aggiungi_stanza(bagno)
appartamentomio.aggiungi_stanza(cucina)
appartamentomio.aggiungi_stanza(salotto)

print(appartamentomio.calcola_superficie())       # invoco il metodo calcola_superficie() dell'appartamento e scrivo il risultato
{% endhighlight %}

Possiamo notare come inizialmente vado a creare una istanza di appartamento invocando il suo costruttore e fornendo a questo una stringha di testo che rappresenta l'indirizzo.
Vado poi a creare una istanza di stanza per ogni stanza. Per ognuna di questa invoco il relativo costruttore fornendo un nome alla stanza, e la sua lunghezza e la larghezza.
Possiamo notare come questa struttura realizzi nella memoria del computer una struttura di dati che **modellizza** la struttura reale.

A questo punto vado a sfruttare la composizione per calcolare l'area dell'appartamento.

### Esempio: la scuola

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

La classe appena definita contiene anche alcuni metodi che lavorano sugli oggetti contenuti. I metodi *conta\_studenti* e *conta\_professori* reestituiscono il numero di studenti e di professori che sono contenuti nelle rispettive liste.

A questo punto è possibile utilizzare la classe appena definita come segue:

{% highlight python %}
patiniliberatore = Scuola()         
        
patiniliberatore.aggiungi_studente(Alunno("Mario", "Rossi")) 
patiniliberatore.aggiungi_studente(Alunno("Rita", "Morelli")) 
patiniliberatore.aggiungi_professore(Professore("Antonino", "Anile"))

print("Nel Patini studiano", patiniliberatore.conta_studenti(), "studenti")
{% endhighlight %}

Notiamo che alla riga 1 creiamo una nuova istanza della classe scuola. Dalla riga 3 alla 5 popoliamo la scuola con due studenti ed un professore. In fine alla riga 7 utilizziamo il metodo *conta\_studenti* per contare il numero di studenti nella scuola.

## Inizia scrivendo un test 

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

Quando vado a lanciare il file questo manderà in esecuzione tutti i test e nel caso uno di questi non funzioni, cioè se la mia implementazione non è

corretta, visualizzerà un messaggio di errore. Questo espediente mi consente di correggere l’implementazione della classe e dei suoi metodi dato che è molto rapido eseguire i test per vedere se tutto funziona come atteso.

Un test rappresenta anche una forma di contratto tra programmatori. Quando due programmatori collaborano su di una applicazione possono accordarsi sulla firma delle classi e dei loro metodi e su alcuni esempi che scrivono sotto forma di test. In questo modo sarà chiaro per entrambi quando il codice è ben implementata.

Ultimo vantaggio: un test mi permette di fare refactoring del codice. Questo significa che posso lavorare sull’implementazione della classe per migliorarla dal punto di visto delle prestazioni per esempio, essendo sicuro di non introdurre bugs nel codice dato che di volta in volta che faccio modifiche controllerò che tutti i test diano esito positivo.

## Scrivere la documentazione per ciascuna classe 

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

## Organizza il tuo codice in moduli 

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

La funzione main contiene il flusso principale del programma.

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

In seguito troviamo un file di libreria in cui abbiamo precedentemente scritto l’implementazione delle funzioni che mi occorrono per fare conversioni tra grandezze. Questo file è contenuto nella cartella grandezze e si chiama conversioni.py. Questo file contiene due funzioni la prima trasforma i gradi Celsius in Fahrenheit, la seconda fa il contrario trasforma i gradi Fahrenheit in gradi Celsius. Notare che nella parte finale dentro l’if ci sono i test che si occupano di assicurarci la buona qualità della libreria. Se mando in run il file conversioni.py e visualizzo la scritta “tutto ok” vuol dire che la libreria funziona come ci si aspetta.

L’esempio che abbiamo appena visto ci dà idea su come gestire il codice in moduli che posso riutilizzare su molte applicazioni.

## Esercizi 


### Creazione di classi

#### Esercizio 1:

Crea una classe Python nominata Impiegato con gli attributi matricola, nome, salario e dipartimento e metodi come calcola_salario, imposta_dipartimento e descrivi_impiegato

Istanze:
"ADAMS", "E7876", 50000, "ACCOUNTING"
"JONES", "E7499", 45000, "RESEARCH"
"MARTIN", "E7900", 50000, "SALES"
"SMITH", "E7698", 55000, "OPERATIONS"
Crea 'imposta_dipartimento' per cambiare l'attributo dipartimento dell'impiegato
Crea 'print_employee_details' per scrivere il contenuto di tutti gli attributi di un impiegato
Crea 'calculate_emp_salary' per calcolare l'ammontare del salario prendendo come parametro le ore lavorate dall'impiegate e aggiungendo se necessario lo straordinario:
straordinario = ore_lavorate - 50
ammontare_straordinario = (straordinario * (salario / 50))

#### Esercizio 2:

Crea una classe Python nominata Lavorazione con gli attributi id, nome, metriQuadrati e costoAlMetro.

Crea il metodo **init** per inzializzare la classe.
Crea il metodo **calcolaCostoTotale** che restituisce il risultato del prodotto degli altributi metriQuadrati e costoAlMetro

Crea le Istanze:

* 1, "Rimozione pavimento", 32, 48
* 2, "Posa del massetto grezzo", 32, 30
* 3, "Posa massetto autolivellante", 32, 80
* 4, "Posa mattonelle", 32, 90

Chiama il metodo **calcolaCostoTotale** per ciascuna istanza.

#### Esercizio 3:

Crea una classe Python nominata AlberoAntico con gli attributi id, nome, nomeEsteso, lat, long, anni

Crea il metodo **init** per inzializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente tutti gli attributi

Crea le Istanze:

* 1, "Abete", "Abies alba", 17.4, 28.3, 145
* 2, "Quercia", "Quercus", 18.4, 27.3, 122
* 3, "Faggio", "Fagus", 17.2, 28.5, 259
* 4, "Ginepro", "Juniperus", 17.1, 23.3, 32

Chiama il metodo **presentazione** per ciascuna istanza.

#### Esercizio 4:

Crea una classe Python nominata Libro con gli attributi isbn, titolo, autore, pagine, editore

Crea il metodo **init** per inzializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente una stringa formattata in questo modo:
il libro *titolo* è stato scritto da *autore* 

All'interno del main:

Crea 4 istanze la prima delle quali deve essere:

* 978-8849419535, "La Divina Commedia", "Dante Alighieri", 427, Petrini
* ....

Inserisci tutte le istanze in una lista e visita la lista chiamando metodo **presentazione** per ciascuna istanza.

#### Esercizio 5:

Crea una classe Python nominata Arredo con gli attributi nrCatalogo, nome, altezza, larghezza, profondita, materiale

Crea il metodo **init** per inzializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente tutti i dati dell'arredo in forma di stringa di testo.
Crea il metodo **calcolaVolume** che restituisce il volume dell'arredo come prodotto di altezza, larghezza, profondita.

All'interno del main:

Crea 4 istanze con i dati:

* 245, "Banco", 55, 45, 30, "Legno e metallo"
* 246, "Banco a rotelle", 60, 45, 55, "Legno, metallo, plastica"
* 247, "Sedia", 65, 40, 55, "Legno, metallo"
* 248, "Cattedra", 65, 140, 100, "Legno, metallo"

Inserisci tutte le istanze in una lista e visita la lista chiamando metodo **presentazione** e **calcolaVolume** per ciascuna istanza.

#### Esercizio 6:

Crea una classe Python nominata Edificio con gli attributi indirizzo, citta, altezza, larghezza, profondita

Crea il metodo **init** per inzializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente tutti i dati dell'edificio in forma di stringa di testo.
Crea il metodo **calcolaVolume** che restituisce il volume dell'edificio come prodotto di altezza, larghezza, profondita.

All'interno del main:

Crea 4 istanze con i dati:

* "Via Verdi nr 2", "Castel di Sangro", 55, 45, 50
* "Via Verdi nr 4", "Castel di Sangro", 55, 25, 40
* "Via Verdi nr 6", "Castel di Sangro", 155, 45, 30
* "Via Verdi nr 8", "Castel di Sangro", 15, 45, 130

Inserisci tutte le istanze in una lista e visita la lista chiamando metodo **presentazione** e **calcolaVolume** per ciascuna istanza.

#### Esercizio 7:

![Panineria](/images/python/oggetti/panineria.png){:class="aside-image"}

Implementare il diagramma UML di fianco creando tutte le classi necessarie. 
Creare le istanze necessarie per un panino al prosciutto condito con olio e maionese e contenuto in un menù che ha come bevanda una cocacola.
Calcolare le calorie del menù seguendo la seguente tabella:

* prosciutto Cal 145 per 100gr
* olio Cal 884 per 100gr
* maionese Cal 680 per 100gr
* coca cola 38 Cal per 100gr



### Esercizio 8:

Fai la tabella di tracciamento:

{% highlight python %}
class Conto:
   def __init__(self):
       self.saldo = 0
   def deposita(self, ammontare):
       self.saldo = self.saldo + ammontare
   def preleva(self, ammontare):
       if self.saldo > ammontare:
         self.saldo = self.saldo - ammontare
       else:
         return "sei povero"
   def estrattoConto(self):
       return self.saldo
       
       
# main
mioconto = Conto()
mioconto.deposita(1000)
print(mioconto.estrattoConto())
mioconto.deposita(2000)
print(mioconto.estrattoConto())
mioconto.preleva(500)
print(mioconto.estrattoConto())

contoBeatrice = Conto()
contoBeatrice.deposita(1000000)
print(contoBeatrice.estrattoConto())
contoBeatrice.preleva(500000)
print(contoBeatrice.estrattoConto())
{% endhighlight %}

### Creazione di strutture basate su ereditarietà


### Creazione di strutture basate su composizione

#### Esercizio 1:
Crea una composizione di oggetti: gli oggetti da comporre sono Albero e Foresta. 
L'labero contiene gli attributi nome e altezza. La foresta contiene gli alberi ed è in grado di calcolare la somma delle altezze di tutti gli alberi al suo interno

#### Esercizio 1:
Completa la struttura descritta all’interno del capitolo aggiungendo altre 5 classi di persone che ruotano nell’ambito del mondo della scuola.

### Esercizio 2:
Crea la struttura di classi necessaria per descrivere gli animali all’interno di uno zoo. Parti dalla classe base Animali e poi definisci le sottoclassi Mammiferi Rettili Uccelli e poi continua creando una classe per ciascun tipo di Animale: Orso, Vipera ecc. fino a 10 animali. Le classi avranno un attributo di classe per il nome scientifico, un attributo di istanza per il nome de singolo e avranno un metodo per presentarsi ed un per fare il verso (Bau bau, Miao, Zzzzz ecc.).

### Esercizio 3:
Immagina di dover fare una applicazione per pagare gli stipendi in una azienda. Il direttore guadagna 100000 euro/anno, il vi- cedirettore 70000, una manager di medio livello 50000, gli impiegati 35000. Le ore di straordinario vengono retribuite il 20% più del normale. Chi fa il part-time guadagna il 60% del suo stipendio lordo. Crea la struttura di classi e i metodi per calcolare gli stipendi del personale in basso.

### Esercizio 4:

Crea una classe automobile che conservi memoria della distanza percorsa dall'acquisto (odometro o conta km).
La classe possiede un metodo "percorri" che prende come parametro i km percorsi da aggiungere alla distanza percorsa dal momento
dell'acquisto.
La classe possiede il metodo "getKm" che restituisce i km percorsi da quando acquistata.
La classe possiede anche il metodo "tarocca" che si preoccupa di far sembrare che la macchina non abbia mai percorso più di 10000 km in modo da poterla rivendere al massimo prezzo possibile

### Esercizio 5:

Fai la tabella di tracciamento:

{% highlight python %}
class Macchina:
    
	def __init__(self, modello):
	    self.modello = modello
	    self.componenti = []
		
	def aggiungiComponente(self, componente):
	    self.componenti.append( componente ) # il metodo append del tipo lista aggiunge un elemento ad una lista
        
	def calcolaPeso(self):
	    peso = 0
	    for componente in self.componenti:
	        peso = peso + componente.getPeso()
	    return peso
    
class Componente:
    
    def __init__(self, nome, peso):
	    self.nome = nome
	    self.peso = peso
    
    def getPeso(self):
	    return self.peso
        

# main
m1 = Macchina("Ford Fiesta")
c1 = Componente("Ammortizzatore", 10)
m1.aggiungiComponente( c1 )
c2 = Componente("Cruscotto", 20)
m1.aggiungiComponente( c2 )
c2 = Componente("Sedile", 12)
print(m1.calcolaPeso())
{% endhighlight %}



