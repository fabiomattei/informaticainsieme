---
title: 'La programmazione ad oggetti'
date: '2026-08-17T10:35:00+01:00'
author: Fabio Mattei
layout: page
---

## Introduzione

Nella programmazione procedurale distinguiamo tra dati e funzioni. I primi costituiscono l'insieme delle informazioni su cui operiamo, le seconde costituiscono l'insieme degli algoritmi che operano sui dati. Nella programmazione tradizionale queste sono due entità distinte.

Quando il codice diventa molto ampio si aprono problematiche come:

- collisioni di nomi: funzioni o variabili che vengono definite in sezioni diverse di programma, da persone diverse, con lo stesso nome. Quando il computer si trova di fronte a queste non sa cosa fare;
- side effects: una variabile viene utilizzata da un programmatore in un modo e da un altro programmatore in un altro, questo crea comportamenti inaspettati da parte del software.

Nella programmazione ad oggetti si cerca di ovviare a queste problematiche creando dei pacchetti di informazioni e di funzioni che operano su queste. I primi si chiamano attributi (o proprietà), i secondi si chiamano metodi.

## Sintassi

La classe più semplice che possiamo definire in PHP è:

{% highlight php %}
<?php
class Test {
}
{% endhighlight %}

Come possiamo vedere dall'esempio, per definire una classe basta usare la parola chiave class, seguita dal nome che vogliamo dare alla classe (in questo caso Test, con la lettera maiuscola per convenzione), seguita da un blocco di codice racchiuso tra graffe (in questo caso vuoto) che è il codice appartenente alla classe stessa.

Proviamo ora a definire una classe Cerchio.

{% highlight php %}
<?php
class Cerchio {

    const PIGRECO = 3.14;

    public $raggio;

    public function __construct($raggio) {
        /* Inizializza la classe
         * parametro raggio int o float
         */
        $this->raggio = $raggio;
    }

    public function calcolaArea() {
        /* Calcola l'area del cerchio
         * return float
         */
        return $this->raggio ** 2 * self::PIGRECO;
    }

    public function calcolaCirconferenza() {
        /* Calcola la circonferenza del cerchio
         * return float
         */
        return 2 * self::PIGRECO * $this->raggio;
    }
}
{% endhighlight %}

La classe Cerchio ha una **costante di classe** chiamata *PIGRECO*. È una costante di classe perché ha lo stesso valore per tutte le istanze della classe e, a differenza di un normale attributo, non può essere modificata dopo la definizione. Per accedervi dall'interno della classe si usa `self::PIGRECO`.

Questo vuol dire che posso definire molti cerchi, ognuno con un raggio diverso da quello degli altri, ma per tutti il valore di π sarà sempre lo stesso.

La classe ha anche un attributo (proprietà) di istanza chiamato $raggio. Questo sarà impostato di volta in volta in modo differente per ciascun cerchio appartenente alla classe.

La classe Cerchio ha due metodi: calcolaArea e calcolaCirconferenza. Questi si occupano di operare sulle informazioni contenute nella classe per implementare gli algoritmi necessari.

Nota che mentre gli attributi di classe sono targhette per identificare le informazioni e dunque sono descritti da sostantivi, i metodi sono targhette per identificare algoritmi e sono dunque descritti da verbi.

## Classi

La classe è un costrutto che ci permette di definire la base per creare oggetti. All'interno di una classe si definisce la struttura delle informazioni su cui questa lavorerà e gli algoritmi che opereranno sulle informazioni stesse. **Le classi sono costrutti astratti, non si riferiscono a nessun oggetto specifico**. Ad esempio posso definire la classe *Alunno* che al suo interno conterrà le stringhe nome e cognome senza entrare nella specificità dell'alunno che si chiama Mario Rossi.

{% highlight php %}
<?php
class Alunno {

    public $nome;
    public $cognome;

    public function __construct($nome, $cognome) {
        /* Costruttore
         * parametro nome string parametro cognome string
         */
        $this->nome = $nome;
        $this->cognome = $cognome;
    }

    public function saluta() {
        /* Scrive una stringa di saluto
         * return string
         */
        return "Ciao, mi chiamo " . $this->nome . " " . $this->cognome;
    }
}
{% endhighlight %}

## Istanze

Le istanze sono oggetti creati a partire da una classe. Sono la materializzazione della classe stessa. Al fine di creare una istanza si utilizza la parola chiave **new** seguita dal nome della classe e, tra parentesi, gli argomenti esplicitati in fase di definizione del costruttore `__construct`.

{% highlight php %}
<?php
$mario = new Alunno("Mario", "Rossi");
echo $mario->saluta();
$rita = new Alunno("Rita", "Morelli");
echo $rita->saluta();
$cerchio5 = new Cerchio(5.5);
echo $cerchio5->calcolaCirconferenza();
{% endhighlight %}

A differenza di Python, dove creare un'istanza si scrive semplicemente `Alunno("Mario", "Rossi")`, in PHP la parola chiave `new` è **sempre obbligatoria**.

## Attributi

Le informazioni contenute in una classe vengono memorizzate attraverso gli attributi (in PHP si chiamano più propriamente **proprietà**). Ad esempio la classe Alunno contiene le proprietà $nome e $cognome. Queste sono accessibili liberamente dai metodi della classe ma è buona abitudine renderle inaccessibili dall'esterno, questo perché un accesso diretto da parte del consumer sugli attributi viene meno il principio di incapsulamento. Esistono due tipi di attributi:

- attributi di classe (costanti, con `const`, o proprietà statiche, con `static`)
- attributi di istanza

Gli attributi di istanza sono informazioni che sono definite istanza per istanza, ad esempio il nome e il cognome della classe Alunno; gli attributi di classe sono invece informazioni comuni a tutte le istanze, per esempio il valore di π per la classe Cerchio. È bene che ciascun attributo sia descritto da un sostantivo.

### Visibilità: public, private, protected

A differenza di Python, dove la privatezza di un attributo è solo una **convenzione** (un underscore davanti al nome), PHP offre un vero controllo di accesso attraverso tre parole chiave:

- **public**: la proprietà (o il metodo) è accessibile da qualsiasi punto del programma, anche dall'esterno della classe
- **private**: la proprietà (o il metodo) è accessibile solo dall'interno della classe stessa
- **protected**: la proprietà (o il metodo) è accessibile dall'interno della classe e dalle sue sottoclassi, ma non dall'esterno

{% highlight php %}
<?php
class Alunno {
    private $nome;      // accessibile solo dentro la classe Alunno
    private $cognome;

    public function __construct($nome, $cognome) {
        $this->nome = $nome;
        $this->cognome = $cognome;
    }

    public function saluta() {
        return "Ciao, mi chiamo " . $this->nome . " " . $this->cognome;
    }
}

$mario = new Alunno("Mario", "Rossi");
echo $mario->saluta();  // corretto, il metodo è public
echo $mario->nome;      // Errore fatale: Cannot access private property
{% endhighlight %}

## Metodi

I metodi sono gli algoritmi che operano con le informazioni contenute negli attributi. Dato che i metodi sono sostanzialmente nomi di algoritmi che "fanno cose" è bene che questi siano dei verbi.

- le definizioni dei metodi si trovano all'interno delle graffe della classe;
- la sintassi usata per definire i metodi è uguale a quella usata per definire le funzioni, preceduta da una parola chiave di visibilità (public, private, protected);
- per chiamare un metodo basta usare la sintassi `$istanza->metodo()`.

## La parola chiave $this

Come si nota negli esempi precedenti, PHP usa `$this` per riferirsi all'istanza corrente, esattamente come Python usa `self`. La differenza fondamentale è che **in Python `self` deve essere dichiarato esplicitamente come primo parametro di ogni metodo**, mentre **in PHP `$this` è sempre disponibile automaticamente dentro un metodo, senza doverlo dichiarare come parametro**.

{% highlight php %}
<?php
class Esempio {
    public $valore;

    public function raddoppia() {
        // $this è già disponibile, non va dichiarato tra i parametri!
        return $this->valore * 2;
    }
}
{% endhighlight %}

Dato che `$this` si riferisce all'istanza, possiamo usarlo per accedere ad altri attributi e metodi definiti all'interno della classe semplicemente facendo `$this->attributo` o `$this->metodo()`. Le classi supportano anche diversi **metodi magici**, identificati dalla presenza di due underscore prima del nome (come `__construct`, `__toString`). Uno di questi metodi speciali è `__construct`, chiamato automaticamente ogni volta che un'istanza viene creata con `new`.

## Incapsulamento

Progettare un software attraverso l'approccio Object Oriented consiste nel trovare informazioni logicamente correlate e inserirle all'interno di una classe. Queste informazioni lavoreranno a stretto contatto le une con le altre attraverso gli algoritmi (metodi) che fanno parte della classe stessa.

Quando si parla di incapsulamento si intendono due aspetti della progettazione del software:

- mettere insieme informazioni e metodi logicamente connessi in una classe;
- nascondere i dettagli implementativi al di fuori della classe stessa (in PHP, con `private` e `protected`, l'incapsulamento è imposto dal linguaggio, non solo suggerito per convenzione).

Pensiamo ad esempio all'automobile. Un'automobile possiede delle proprietà come: nome, modello, azienda produttrice, tipo di motore, cilindrata ecc. L'automobile inoltre può fare delle azioni come: apri le porte, spostati, fai scattare l'allarme ecc. Queste proprietà e questi metodi, dato che sono **correlati**, sono insieme all'interno della classe Automobile.

## Ereditarietà

La programmazione ad oggetti permette di creare gerarchie di classi che condividono metodi ed attributi al fine di permettere al programmatore di definire di volta in volta non l'intero ammontare di informazioni e algoritmi ma soltanto ciò che varia.

Supponiamo di aver definito due classi Docente ed Alunno che definiscono il metodo **saluta** e il metodo **lavora**. Il metodo saluta è analogo per entrambe le classi mentre il metodo lavora è diverso. Entrambe le classi contengono gli attributi nome e cognome ma la classe Docente contiene l'attributo materia mentre la classe Alunno contiene l'attributo classe.

{% highlight php %}
<?php
class Docente {
    public $nome;
    public $cognome;
    public $materia;

    public function __construct($nome, $cognome, $materia) {
        $this->nome = $nome;
        $this->cognome = $cognome;
        $this->materia = $materia;
    }

    public function saluta() {
        return "Ciao, mi chiamo " . $this->nome . " " . $this->cognome;
    }

    public function lavora() {
        return "Sto insegnando " . $this->materia;
    }
}

class Alunno {
    public $nome;
    public $cognome;
    public $classe;

    public function __construct($nome, $cognome, $classe) {
        $this->nome = $nome;
        $this->cognome = $cognome;
        $this->classe = $classe;
    }

    public function saluta() {
        return "Ciao, mi chiamo " . $this->nome . " " . $this->cognome;
    }

    public function lavora() {
        return "Sto frequentando la classe " . $this->classe;
    }
}
{% endhighlight %}

L'ereditarietà mi permette di astrarre in una classe genitore le informazioni e i metodi comuni in modo da rendere il codice molto più compatto.

Definiamo dunque la classe Persona che contiene gli attributi **nome** e **cognome** e contiene il metodo **saluta**. Sono stati scelti questi attributi e questo metodo perché comuni sia alla classe Docente che alla classe Alunno.

Quando vado a definire la classe Docente indico il fatto che questa eredita da Persona con la parola chiave **extends**, messa dopo il nome della classe Docente. A questo punto la classe Docente erediterà da Persona attributi e metodi e le sue implementazioni potranno utilizzare questi attributi e questi metodi ereditati come se fossero parte integrante della classe Docente. Mi comporto analogamente con la classe Alunno.

Notiamo che il costruttore della classe Docente è un po' diverso dal solito. Questo avviene perché vado a richiamare al suo interno il costruttore della classe padre attraverso `parent::__construct(...)`, l'equivalente PHP di `super().__init__(...)` in Python, in modo da non implementare due volte l'assegnazione degli attributi nome e cognome.

{% highlight php %}
<?php
class Persona {
    public $nome;
    public $cognome;

    public function __construct($nome, $cognome) {
        $this->nome = $nome;
        $this->cognome = $cognome;
    }

    public function saluta() {
        return "Ciao, mi chiamo " . $this->nome . " " . $this->cognome;
    }
}

class Docente extends Persona {
    public $materia;

    public function __construct($nome, $cognome, $materia) {
        parent::__construct($nome, $cognome);
        $this->materia = $materia;
    }

    public function lavora() {
        return "Sto insegnando " . $this->materia;
    }
}

class Alunno extends Persona {
    public $classe;

    public function __construct($nome, $cognome, $classe) {
        parent::__construct($nome, $cognome);
        $this->classe = $classe;
    }

    public function lavora() {
        return "Sto frequentando la classe " . $this->classe;
    }
}
{% endhighlight %}

Notiamo come il codice diventa più snello dato che non ho bisogno di definire due volte gli stessi metodi e gli stessi attributi, riducendo la ridondanza del codice.

A questo punto posso definire il main in questo modo:

{% highlight php %}
<?php
$mario = new Alunno("Mario", "Rossi", "4C");
$rosa = new Alunno("Rosa", "Verdi", "3C");
$armando = new Docente("Armando", "Bianchi", "Filosofia");

echo $mario->saluta(), "\n";
echo $mario->lavora(), "\n";

echo $rosa->saluta(), "\n";
echo $rosa->lavora(), "\n";

echo $armando->saluta(), "\n";
echo $armando->lavora(), "\n";
{% endhighlight %}

## Polimorfismo

Si parla di polimorfismo quando ho oggetti di tipo differente (appartengono a classi diverse) che hanno la stessa interfaccia.

Spieghiamo meglio il concetto: l'interfaccia di un oggetto (che ricordiamo è l'istanza di una classe) è rappresentata dai metodi che sono definiti nella sua classe. Quindi se due classi diverse implementano soltanto metodi aventi lo stesso nome, con gli stessi parametri, allora le classi hanno la stessa interfaccia.

I metodi che vengono ridefiniti in una sottoclasse sono detti **polimorfi**, in quanto lo stesso metodo si comporta diversamente a seconda della classe a cui appartiene l'oggetto dal quale è invocato.

L'interfaccia di una classe definisce un **contratto** generale che sottoclassi diverse possono soddisfare in modi diversi. PHP mette anche a disposizione la parola chiave **interface** per definire esplicitamente questi contratti, ma non è indispensabile per ottenere il polimorfismo, esattamente come in Python.

### Esempio: geometria

{% highlight php %}
<?php
class Triangolo {
    public $base;
    public $altezza;

    public function __construct($base, $altezza) {
        // Costruttore della classe Triangolo
        $this->base = $base;
        $this->altezza = $altezza;
    }

    public function calcolaArea() {
        // Metodo che restituisce l'area del triangolo
        return $this->base * $this->altezza / 2;
    }
}
{% endhighlight %}

{% highlight php %}
<?php
class Rettangolo {
    public $base;
    public $altezza;

    public function __construct($base, $altezza) {
        // Costruttore della classe Rettangolo
        $this->base = $base;
        $this->altezza = $altezza;
    }

    public function calcolaArea() {
        // Metodo che restituisce l'area del rettangolo
        return $this->base * $this->altezza;
    }
}
{% endhighlight %}

Notiamo che le classi appena definite implementano metodi con lo stesso nome e con gli stessi parametri, dunque hanno la stessa interfaccia.

A questo punto nel **main** posso scrivere:

{% highlight php %}
<?php
$t1 = new Triangolo(2, 4);   // creo una istanza di Triangolo
$t2 = new Triangolo(2, 6);   // creo una seconda istanza di Triangolo
$r1 = new Rettangolo(2, 3);  // creo una istanza di Rettangolo

echo $t1->calcolaArea(), "\n"; // invoco il metodo calcolaArea() appartenente all'istanza t1 della classe Triangolo
echo $t2->calcolaArea(), "\n"; // invoco il metodo calcolaArea() appartenente all'istanza t2 della classe Triangolo
echo $r1->calcolaArea(), "\n"; // invoco il metodo calcolaArea() appartenente all'istanza r1 della classe Rettangolo
{% endhighlight %}

### Esempio: la scuola

{% highlight php %}
<?php
$scuola = [];
$scuola[] = new Alunno("Mario", "Rossi", "4C");
$scuola[] = new Alunno("Rita", "Morelli", "3C");
$scuola[] = new Docente("Antonino", "Anile", "Storia");
foreach ($scuola as $persona) {
    echo $persona->lavora(), "\n";
}
{% endhighlight %}

Le istanze di una sottoclasse possono essere utilizzate al posto delle istanze della superclasse. L'overriding dei metodi o delle proprietà permette che gli oggetti appartenenti alle sottoclassi di una stessa classe rispondano diversamente agli stessi utilizzi.

Per esempio nell'esempio in alto abbiamo tre oggetti, i primi due appartenenti alla classe Alunno e il terzo appartenente alla classe Docente. Dato che tutti questi oggetti definiscono il metodo lavora è possibile scrivere un costrutto come quello contenuto nel ciclo foreach.

## Composizione

Negli esempi che abbiamo visto finora gli attributi delle classi erano variabili di tipo primitivo; è però possibile definire come attributi dei riferimenti ad oggetti di un'altra classe. In questo modo abbiamo oggetti composti da altri oggetti.

### Esempio: l'appartamento

Facciamo un esempio. Immaginiamo di voler calcolare la superficie di un appartamento. L'appartamento è composto da due camere da letto, un bagno, una cucina ed un salotto.

Se ci pensiamo un attimo notiamo che tutti questi ambienti sono schematizzabili come rettangoli. Possiamo dunque definire una classe in questo modo:

{% highlight php %}
<?php
class Stanza {
    public $nome;
    public $lunghezza;
    public $larghezza;

    public function __construct($nome, $lunghezza, $larghezza) {
        // Costruttore della classe Stanza
        $this->nome = $nome;
        $this->lunghezza = $lunghezza;
        $this->larghezza = $larghezza;
    }

    public function calcolaSuperficie() {
        // Metodo che restituisce la superficie della stanza
        return $this->lunghezza * $this->larghezza;
    }
}
{% endhighlight %}

La nostra classe **Stanza** possiede tre attributi: il nome, la lunghezza e la larghezza. Una stanza è inoltre in grado di **calcolare la sua superficie** attraverso un metodo che restituisce il prodotto dell'attributo larghezza moltiplicato per l'attributo lunghezza.

A questo punto possiamo implementare l'appartamento come un contenitore di stanze:

{% highlight php %}
<?php
class Appartamento {
    public $indirizzo;
    public $stanze;

    public function __construct($indirizzo) {
        // Costruttore della classe Appartamento
        $this->indirizzo = $indirizzo;
        $this->stanze = [];
    }

    public function aggiungiStanza($stanza) {
        // Questo metodo aggiunge la stanza passata come parametro
        // all'array di stanze contenute nell'appartamento
        $this->stanze[] = $stanza;
    }

    public function calcolaSuperficie() {
        // Calcola la superficie dell'intero appartamento
        // sommando tra loro le superfici delle singole stanze
        $superficie = 0;
        foreach ($this->stanze as $stanza) {
            $superficie = $superficie + $stanza->calcolaSuperficie();
        }
        return $superficie;
    }
}
{% endhighlight %}

A questo punto possiamo implementare il **main** in questo modo:

{% highlight php %}
<?php
$appartamentomio = new Appartamento("via Mazzini, 22"); // creo una istanza di appartamento
$camera_grande = new Stanza("Camera grande", 5, 5);     // creo una istanza di stanza per la camera grande
$camera_piccola = new Stanza("Camera piccola", 4, 5);   // creo una istanza di stanza per la camera piccola
$bagno = new Stanza("Bagno", 4, 2);                     // creo una istanza di stanza per il bagno
$cucina = new Stanza("Cucina", 4, 2);                   // creo una istanza di stanza per la cucina
$salotto = new Stanza("Salotto", 4, 2);                 // creo una istanza di stanza per il salotto
$appartamentomio->aggiungiStanza($camera_grande);       // aggiungo la camera grande al mio appartamento
$appartamentomio->aggiungiStanza($camera_piccola);
$appartamentomio->aggiungiStanza($bagno);
$appartamentomio->aggiungiStanza($cucina);
$appartamentomio->aggiungiStanza($salotto);

echo $appartamentomio->calcolaSuperficie(); // invoco il metodo calcolaSuperficie() dell'appartamento e scrivo il risultato
{% endhighlight %}

Possiamo notare come inizialmente vado a creare una istanza di appartamento invocando il suo costruttore e fornendo a questo una stringa di testo che rappresenta l'indirizzo.
Vado poi a creare una istanza di stanza per ogni stanza. Per ognuna di queste invoco il relativo costruttore fornendo un nome alla stanza, e la sua lunghezza e la larghezza.
Possiamo notare come questa struttura realizzi nella memoria del computer una struttura di dati che **modellizza** la struttura reale.

A questo punto vado a sfruttare la composizione per calcolare l'area dell'appartamento.

### Esempio: la scuola

Creiamo una classe scuola che contenga al suo interno un array di studenti, un array di professori, un array di persone che compongono il personale ATA ed un preside.

{% highlight php %}
<?php
class Scuola {
    public $preside;
    public $alunni;
    public $professori;
    public $personaleAta;

    public function __construct() {
        $this->preside = null;
        $this->alunni = [];
        $this->professori = [];
        $this->personaleAta = [];
    }

    public function aggiungiStudente($studente) {
        $this->alunni[] = $studente;
    }

    public function definisciPreside($preside) {
        $this->preside = $preside;
    }

    public function aggiungiProfessore($professore) {
        $this->professori[] = $professore;
    }

    public function aggiungiAta($ata) {
        $this->personaleAta[] = $ata;
    }

    public function contaStudenti() {
        return count($this->alunni);
    }

    public function contaProfessori() {
        return count($this->professori);
    }
}
{% endhighlight %}

La classe appena definita contiene anche alcuni metodi che lavorano sugli oggetti contenuti. I metodi *contaStudenti* e *contaProfessori* restituiscono il numero di studenti e di professori che sono contenuti nei rispettivi array.

A questo punto è possibile utilizzare la classe appena definita come segue:

{% highlight php %}
<?php
$patiniliberatore = new Scuola();

$patiniliberatore->aggiungiStudente(new Alunno("Mario", "Rossi", "4C"));
$patiniliberatore->aggiungiStudente(new Alunno("Rita", "Morelli", "3C"));
$patiniliberatore->aggiungiProfessore(new Docente("Antonino", "Anile", "Storia"));

echo "Nel Patini studiano ", $patiniliberatore->contaStudenti(), " studenti";
{% endhighlight %}

Notiamo che alla riga 1 creiamo una nuova istanza della classe scuola. Dalla riga 3 alla 5 popoliamo la scuola con due studenti ed un professore. Infine utilizziamo il metodo *contaStudenti* per contare il numero di studenti nella scuola.

## Inizia scrivendo un test

Supponiamo di voler scrivere una classe che si occupi di trasformare i gradi Celsius in gradi Fahrenheit. Noi tutti sappiamo che: *Tf = Tc * 9 / 5 + 32*

Ci serviamo dell'istruzione **assert**, esattamente come abbiamo fatto per le funzioni.

{% highlight php %}
<?php
class Conversioni {
    public function trasfCelsInFahr($temp) {
        return $temp * 9 / 5 + 32;
    }

    public function trasfFahrInCels($temp) {
        // da implementare
    }
}

$conv = new Conversioni();
assert($conv->trasfCelsInFahr(0) == 32.0);
assert($conv->trasfCelsInFahr(20) == 68.0);
assert($conv->trasfCelsInFahr(30) == 86.0);
echo "Tutto ok!";
{% endhighlight %}

Quando vado a lanciare il file questo manderà in esecuzione tutti i test e nel caso uno di questi non funzioni, cioè se la mia implementazione non è corretta, `assert()` genererà un errore. Questo espediente mi consente di correggere l'implementazione della classe e dei suoi metodi dato che è molto rapido eseguire i test per vedere se tutto funziona come atteso.

## Scrivere la documentazione per ciascuna classe

È molto importante lasciare traccia su quanto viene implementato. Usiamo lo stesso stile PHPDoc visto per le funzioni:

{% highlight php %}
<?php
/**
 * Questa classe implementa alcune trasformazioni
 * tra diverse scale di temperatura
 */
class Conversioni {

    /**
     * Trasforma la temperatura data in gradi Celsius
     * in temperatura in gradi Fahrenheit
     *
     * @param float $temp
     * @return float
     */
    public function trasfCelsInFahr($temp) {
        return $temp * 9 / 5 + 32;
    }

    public function trasfFahrInCels($temp) {
        // da implementare
    }
}
{% endhighlight %}

## Organizza il tuo codice in file diversi

Esattamente come per le funzioni, organizziamo il codice in più file usando `require_once`.

- progettodifisica/
- progettodifisica/principale.php
- progettodifisica/grandezze/conversioni.php

{% highlight php %}
<?php
// file: principale.php

require_once 'grandezze/conversioni.php';

function main() {
    echo "Temp Celsius: ";
    $temp = (float) trim(fgets(STDIN));
    $conv = new Conversioni();
    $fahr = $conv->trasfCelsInFahr($temp);
    echo 'Temp. Fahrenheit = ' . $fahr;
}

main();
{% endhighlight %}

{% highlight php %}
<?php
// file: grandezze/conversioni.php

class Conversioni {

    public function trasfCelsInFahr($temp) {
        return $temp * 9 / 5 + 32;
    }

    public function trasfFahrInCels($temp) {
        // da implementare
    }
}
{% endhighlight %}

L'esempio che abbiamo appena visto ci dà idea su come gestire il codice in più file che posso riutilizzare su molte applicazioni.

## Esercizi


### Creazione di classi

#### Esercizio 1:

Crea una classe PHP nominata Impiegato con le proprietà matricola, nome, salario e dipartimento e metodi come calcolaSalario, impostaDipartimento e descriviImpiegato

Istanze:
"ADAMS", "E7876", 50000, "ACCOUNTING"
"JONES", "E7499", 45000, "RESEARCH"
"MARTIN", "E7900", 50000, "SALES"
"SMITH", "E7698", 55000, "OPERATIONS"
Crea 'impostaDipartimento' per cambiare l'attributo dipartimento dell'impiegato
Crea 'descriviImpiegato' per scrivere il contenuto di tutte le proprietà di un impiegato
Crea 'calcolaSalario' per calcolare l'ammontare del salario prendendo come parametro le ore lavorate dall'impiegato e aggiungendo se necessario lo straordinario:
straordinario = ore_lavorate - 50
ammontare_straordinario = (straordinario * (salario / 50))

#### Esercizio 2:

Crea una classe PHP nominata Lavorazione con le proprietà id, nome, metriQuadrati e costoAlMetro.

Crea il metodo **__construct** per inizializzare la classe.
Crea il metodo **calcolaCostoTotale** che restituisce il risultato del prodotto delle proprietà metriQuadrati e costoAlMetro

Crea le Istanze:

* 1, "Rimozione pavimento", 32, 48
* 2, "Posa del massetto grezzo", 32, 30
* 3, "Posa massetto autolivellante", 32, 80
* 4, "Posa mattonelle", 32, 90

Chiama il metodo **calcolaCostoTotale** per ciascuna istanza.

#### Esercizio 3:

Crea una classe PHP nominata AlberoAntico con le proprietà id, nome, nomeEsteso, lat, long, anni

Crea il metodo **__construct** per inizializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente tutte le proprietà

Crea le Istanze:

* 1, "Abete", "Abies alba", 17.4, 28.3, 145
* 2, "Quercia", "Quercus", 18.4, 27.3, 122
* 3, "Faggio", "Fagus", 17.2, 28.5, 259
* 4, "Ginepro", "Juniperus", 17.1, 23.3, 32

Chiama il metodo **presentazione** per ciascuna istanza.

#### Esercizio 4:

Crea una classe PHP nominata Libro con le proprietà isbn, titolo, autore, pagine, editore

Crea il metodo **__construct** per inizializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente una stringa formattata in questo modo:
il libro *titolo* è stato scritto da *autore*

All'interno del main:

Crea 4 istanze la prima delle quali deve essere:

* 978-8849419535, "La Divina Commedia", "Dante Alighieri", 427, Petrini
* ....

Inserisci tutte le istanze in un array e visita l'array chiamando il metodo **presentazione** per ciascuna istanza.

#### Esercizio 5:

Crea una classe PHP nominata Arredo con le proprietà nrCatalogo, nome, altezza, larghezza, profondita, materiale

Crea il metodo **__construct** per inizializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente tutti i dati dell'arredo in forma di stringa di testo.
Crea il metodo **calcolaVolume** che restituisce il volume dell'arredo come prodotto di altezza, larghezza, profondita.

All'interno del main:

Crea 4 istanze con i dati:

* 245, "Banco", 55, 45, 30, "Legno e metallo"
* 246, "Banco a rotelle", 60, 45, 55, "Legno, metallo, plastica"
* 247, "Sedia", 65, 40, 55, "Legno, metallo"
* 248, "Cattedra", 65, 140, 100, "Legno, metallo"

Inserisci tutte le istanze in un array e visita l'array chiamando i metodi **presentazione** e **calcolaVolume** per ciascuna istanza.

#### Esercizio 6:

Crea una classe PHP nominata Edificio con le proprietà indirizzo, citta, altezza, larghezza, profondita

Crea il metodo **__construct** per inizializzare la classe.
Crea il metodo **presentazione** che restituisce una stringa di testo contenente tutti i dati dell'edificio in forma di stringa di testo.
Crea il metodo **calcolaVolume** che restituisce il volume dell'edificio come prodotto di altezza, larghezza, profondita.

All'interno del main:

Crea 4 istanze con i dati:

* "Via Verdi nr 2", "Castel di Sangro", 55, 45, 50
* "Via Verdi nr 4", "Castel di Sangro", 55, 25, 40
* "Via Verdi nr 6", "Castel di Sangro", 155, 45, 30
* "Via Verdi nr 8", "Castel di Sangro", 15, 45, 130

Inserisci tutte le istanze in un array e visita l'array chiamando i metodi **presentazione** e **calcolaVolume** per ciascuna istanza.

#### Esercizio 7:

Implementare una gerarchia di classi per una panineria: un Panino ha un nome ed è composto da ingredienti (con le rispettive calorie per 100gr), un Menù ha un panino e una bevanda.
Creare le istanze necessarie per un panino al prosciutto condito con olio e maionese e contenuto in un menù che ha come bevanda una cocacola.
Calcolare le calorie del menù seguendo la seguente tabella:

* prosciutto Cal 145 per 100gr
* olio Cal 884 per 100gr
* maionese Cal 680 per 100gr
* coca cola 38 Cal per 100gr

### Esercizio 8:

Fai la tabella di tracciamento:

{% highlight php %}
<?php
class Conto {
    private $saldo;

    public function __construct() {
        $this->saldo = 0;
    }

    public function deposita($ammontare) {
        $this->saldo = $this->saldo + $ammontare;
    }

    public function preleva($ammontare) {
        if ($this->saldo > $ammontare) {
            $this->saldo = $this->saldo - $ammontare;
        } else {
            return "sei povero";
        }
    }

    public function estrattoConto() {
        return $this->saldo;
    }
}

// main
$mioconto = new Conto();
$mioconto->deposita(1000);
echo $mioconto->estrattoConto(), "\n";
$mioconto->deposita(2000);
echo $mioconto->estrattoConto(), "\n";
$mioconto->preleva(500);
echo $mioconto->estrattoConto(), "\n";

$contoBeatrice = new Conto();
$contoBeatrice->deposita(1000000);
echo $contoBeatrice->estrattoConto(), "\n";
$contoBeatrice->preleva(500000);
echo $contoBeatrice->estrattoConto(), "\n";
{% endhighlight %}

### Creazione di strutture basate su ereditarietà


### Creazione di strutture basate su composizione

#### Esercizio 1:
Crea una composizione di oggetti: gli oggetti da comporre sono Albero e Foresta.
L'albero contiene le proprietà nome e altezza. La foresta contiene gli alberi ed è in grado di calcolare la somma delle altezze di tutti gli alberi al suo interno

#### Esercizio 1:
Completa la struttura descritta all'interno del capitolo aggiungendo altre 5 classi di persone che ruotano nell'ambito del mondo della scuola.

### Esercizio 2:
Crea la struttura di classi necessaria per descrivere gli animali all'interno di uno zoo. Parti dalla classe base Animale e poi definisci le sottoclassi Mammifero, Rettile, Uccello e poi continua creando una classe per ciascun tipo di Animale: Orso, Vipera ecc. fino a 10 animali. Le classi avranno una costante di classe per il nome scientifico, una proprietà di istanza per il nome del singolo esemplare e avranno un metodo per presentarsi ed uno per fare il verso (Bau bau, Miao, Zzzzz ecc.).

### Esercizio 3:
Immagina di dover fare una applicazione per pagare gli stipendi in una azienda. Il direttore guadagna 100000 euro/anno, il vicedirettore 70000, una manager di medio livello 50000, gli impiegati 35000. Le ore di straordinario vengono retribuite il 20% più del normale. Chi fa il part-time guadagna il 60% del suo stipendio lordo. Crea la struttura di classi e i metodi per calcolare gli stipendi del personale in basso.

### Esercizio 4:

Crea una classe automobile che conservi memoria della distanza percorsa dall'acquisto (odometro o conta km).
La classe possiede un metodo "percorri" che prende come parametro i km percorsi da aggiungere alla distanza percorsa dal momento dell'acquisto.
La classe possiede il metodo "getKm" che restituisce i km percorsi da quando acquistata.
La classe possiede anche il metodo "tarocca" che si preoccupa di far sembrare che la macchina non abbia mai percorso più di 10000 km in modo da poterla rivendere al massimo prezzo possibile

### Esercizio 5:

Fai la tabella di tracciamento:

{% highlight php %}
<?php
class Macchina {

    public $modello;
    public $componenti;

    public function __construct($modello) {
        $this->modello = $modello;
        $this->componenti = [];
    }

    public function aggiungiComponente($componente) {
        $this->componenti[] = $componente; // aggiunge un elemento all'array
    }

    public function calcolaPeso() {
        $peso = 0;
        foreach ($this->componenti as $componente) {
            $peso = $peso + $componente->getPeso();
        }
        return $peso;
    }
}

class Componente {

    public $nome;
    public $peso;

    public function __construct($nome, $peso) {
        $this->nome = $nome;
        $this->peso = $peso;
    }

    public function getPeso() {
        return $this->peso;
    }
}

// main
$m1 = new Macchina("Ford Fiesta");
$c1 = new Componente("Ammortizzatore", 10);
$m1->aggiungiComponente($c1);
$c2 = new Componente("Cruscotto", 20);
$m1->aggiungiComponente($c2);
$c2 = new Componente("Sedile", 12);
echo $m1->calcolaPeso();
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 6:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
class Cerchio {
    public $raggio;

    public function __construct($raggio) {
        $this->raggio = $raggio;

    public function calcolaArea() {
        return $this->raggio ** 2 * 3.14;
    }
}
{% endhighlight %}

#### Esercizio 7:

Questo programma genera un errore fatale (`Error: Using $this when not in object context`): individuate quale metodo lo causa e spiegate perché, ricordando che `$this` è disponibile solo dentro metodi chiamati su un'istanza.

{% highlight php %}
<?php
class Cerchio {
    public $raggio;

    public function __construct($raggio) {
        $this->raggio = $raggio;
    }

    public static function calcolaArea() {
        return $this->raggio ** 2 * 3.14;
    }
}

$c = new Cerchio(5);
echo $c->calcolaArea();
{% endhighlight %}

#### Esercizio 8:

Questo programma genera un errore fatale (`Error: Call to undefined method`): individuate quale istruzione lo causa e spiegate perché.

{% highlight php %}
<?php
class Alunno {
    public $nome;
    public $cognome;

    public function __construct($nome, $cognome) {
        $this->nome = $nome;
        $this->cognome = $cognome;
    }
}

$alunno = new Alunno("Mario", "Rossi");
echo $alunno->saluta();
{% endhighlight %}

#### Esercizio 9:

Questo programma non genera errori ma contiene un **errore logico**: tutte le istanze di `Cerchio` finiscono per condividere lo stesso raggio invece di avere ciascuna il proprio. Individuate l'errore.

{% highlight php %}
<?php
class Cerchio {
    public static $raggio = 0;

    public function __construct($raggio) {
        self::$raggio = $raggio;
    }

    public function calcolaArea() {
        return self::$raggio ** 2 * 3.14;
    }
}

$c1 = new Cerchio(5);
$c2 = new Cerchio(10);
echo $c1->calcolaArea();
{% endhighlight %}

#### Esercizio 10:

Questo programma non genera errori ma contiene un **errore logico**: il metodo `deposita` non modifica realmente il saldo del conto. Individuate l'errore.

{% highlight php %}
<?php
class Conto {
    public $saldo;

    public function __construct() {
        $this->saldo = 0;
    }

    public function deposita($ammontare) {
        $saldo = $this->saldo + $ammontare;
    }
}

$mioconto = new Conto();
$mioconto->deposita(1000);
echo $mioconto->saldo;
{% endhighlight %}
