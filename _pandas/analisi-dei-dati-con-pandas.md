---
title: 'Analisi dei dati con Pandas'
date: '2021-09-15T08:04:41+02:00'
author: Fabio Mattei
layout: page
---

Pandas è una libreria Python focalizzata sulla manipolazione e l’analisi dei dati. I dati sono la colonna portante di tutta la teoria dell’informazione e [Pandas](https://pandas.pydata.org/) si rivela spesso una risorsa preziosa.

# Perché Pandas?

[Pandas](https://pandas.pydata.org/) è una libreria open-source scritta in Python fondata su [NumPy](https://numpy.org/) (una libreria Python per il calcolo scientifico) ed ha molte funzionalità per pulire analizzare e manipolare i dati. Lo scopo è quello di estrarre conoscenza dai dati grezzi.

La struttura principale in The main data structure in *[Pandas](https://pandas.pydata.org/) è una tabella bidimensioinale chiamata **DataFrame**. Si possono creare dataframe a partire da vari formati come *CSV*, *XLSX*, *JSON*, *SQL*.

# Iniziamo

Come prima cosa dobbiamo istallare *Pandas.* Ci sono molti ambienti in cui possiamo utilizzarlo. La cosa più semplice da fare è istallare [Anaconda](https://www.anaconda.com/), una distribuzione di software che mira a coprire tutti i bisogni del calcolo scientifico con centinaia di pacchetti preistallati. Anaconda può essere istallata su Windows, macOS, e Linux.

Ad ogni modo forse la cosa più semplice è utilizzare il proprio browser con [Jupyter Notebook](https://jupyter.org/) nel cloud. Si può utilizzare [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio) oppure [Google Colab](https://colab.research.google.com/). Entrambi si possono utilizzare gratis e vengono forniti con centinaia di pacchetti Python preistallati.

Negli esempi utilizzeremo **Google Colab** perché molto semplice da utilizzare e non ha bisogno di istallazione.

## Istallazione ed importazione

Al fine di istallare Pandas sul proprio sistema possiamo utilizzare il gestore nativo di pacchetti Python: pip.


{% highlight html %}
pip install pandas
{% endhighlight %}

oppure


{% highlight html %}
conda install panda
{% endhighlight %}

Nota che se stai usando Google Colab non avrai bisogno di dare questi comandi dati che Pandas è preistallato.

Per utilizzare la libreria Pandas all’interno di uno script scriviamo:


{% highlight html %}
import pandas as pd


{% endhighlight %}

Di solito Pandas viene importato come “pd” per brevità, in modo da non dever scrivere il nome per intero ogni volta che lo si utilizza.

# Pandas DataFrame

Dopo che abbiamo istallato ed importato *Pandas*, vediamo come fare per leggere un file e creare un **Pandas DataFrame**. I dati su cui lavoreremo sono una versione semplificata di una competizione [Kaggle](https://www.kaggle.com/c/home-data-for-ml-course/overview) che riguarda i prezzi delle case.

Il file che contiene i dati è un file di tipo **.CSV** (Comma-separated values) che potete trovare [qui](http://esercizidiinformatica.it/daticase.csv).

## Leggere il file e creare il DataFrame

A fine di leggere il file e inserire i dati in questo contenuti all’interno di un DataFrame untilizziamo il comando read\_csv.

{% highlight python %}
import pandas as pd

PATH = 'https://www.esercizidiinformatica.it/daticase.csv'
df = pd.read_csv(PATH)

{% endhighlight %}

Abbiamo utilizzato il metodo *read\_csv* perché stiamo lavorando con un file csv. Come abbiamo detto prima PANDAS può lavorare con diversi formati di file. Si rimanda alla [documentazione](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html) per vedere quali.

Il medoto *read\_csv* carica in memoria il contenuto di un file e crea un DataFame ma se vogliamo creare un DataFrame a partire da una truttura dati base di Python come **dizionari**, **liste**, **NumPy Array**, o perfino un altro **DataFrame**, potete usare il costruttore della classe DataFrame in questo modo:

{% highlight python %}
df = pd.DataFrame(mydict)


{% endhighlight %}

Controlliamo il tipo di Dataframe che abbiamo appena creato:

{% highlight python %}
type(df)


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/514/1*8OHeUqQ5rZxzPxdCNpMo1Q.png)</figure># Esaminiamo il DataFrame

Diamo una prima occhiata al DataFrame. Possiamo controllare le prime *n* righe del file appena caricato con il metodo *head*. Se *n* non viene specificato si applica il valore di default pari a 5.

{% highlight python %}
df.head()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*P9dcG0lJvmZlftEp2mmRPg.png)</figure>Ad un prima occhiata tutto sembra in ordine. Possiamo allo stesso modo usare il metodo tail per vedere le ultime 5 righe del file:

{% highlight python %}
df.tail()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*gv18jwruP61WZ4XLNd89ag.png)</figure>Ora controlliamo le dimensione del nostro DataFrame stampando l’attributo *shape*.

{% highlight python %}
df.shape


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/260/1*7qFdFOwoju3_MNxemilRdg.png)</figure>Viene restituita una tupla contenente il numero delle righe e delle colonne del nostro Dataframe. Nel nostro caso 1,460 righe e 16 colonne o **features** (caratteristiche)

Possiamo vedere un sommario del Dataframe con il metodo *info*.

{% highlight python %}
df.info()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/772/1*dYOOxU-MzmUojyKhi6TQnQ.png)</figure>Vengono mostrate informazioni utili sul DataFrame, come i nomi delle colonne, il numero di valori non nulli, i tipi di dato, e la quantità di memoria utilizzata. Possiamo vedere che alcune colonne hanno valori mancanti. Approfondiremo questo aspetto più avanti.

Il metodo describe ci fornisce qualche valore statistico sul Dataframe.

{% highlight python %}
df.describe()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/1400/1*VN54H9tL-LPMmV2VuYHmmQ.png)</figure>Viene mostrato il conteggio dei valori, la media, la mediana, la deviazione standard i valori minimi e massimi e la distribuzione nei quartili per ciascuna colonna o feature. Attenzione si mostrano soltanto dati sulle colonne numeriche.

Vediamo un ultimo metodo prima di spostarci alla prossima sezione: *value\_counts*.

{% highlight python %}
df['Neighborhood'].value_counts()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/752/1*u_E9AjnF59x9_m0Y1tZwOg.png)</figure>Questo metodo restituisce una Serie di valori che contiene il numero di valori unici per ciascuna colonna. La si può applicare all’intero DataFrame ma nell’esempio l’abbiamo applicata soltanto alla colonna “Neighborhood”.

Prima di andare avanti cerchiamo di capire con che dati abbiamo a che fare:

- *Id* — Identificatore unico per ciascuna riga (indice).
- *LotArea* — Grandezza del parcheggio in piedi quadrati (sono dati americani)
- *Street* — Tipo di accesso alla strada
- *Neighborhood* — Allocazione fisica della casa
- *HouseStyle* — Stile di residenza
- *YearBuilt* — Data di costruione
- *CentralAir* — Aria condizionata centralizzata
- *BedroomAbvGr* — Numero di camere da letto al di sopra del piano terra
- *Fireplaces* — Numero di caminetti
- *GarageType* — Posizione del Garage
- *GarageYrBlt* — Anno di costruzione del garage
- *GarageArea* — Grandezza del garage in piedi quadrati
- *PoolArea* — Grandezza della piscina in piedi quadrati
- *PoolQC* — Qualità della piscina
- *Fence* — Qualità della recinzione
- *SalePrice* — Prezzo di vendita

Il nostro dataset contiene dati di diverso tipo: numerici, categorie, booleani.

Ora cominciamo a manipolare il Dataframe.

# Manipoliamo i dati con Pandas

*Pandas* ci fornisce molti strumenti per manipolare i dati. In questa sezione vedremo come manipolare righe e colonne inoltre vedremo come localizzare i dati e modificarli. Iniziamo creando un indice per il nostro DataFrame.

## L’indice del DataFrame

Quando abbiamo visualizzato i nostri dati ci siamo accorti che la prima colonna (**Id**) ha un valore unico per ciascuna riga. Noi possiamo avvantaggiarci di questa caratteristica e utilizzare questa colonna come indice al posto dell’indice creato automaticamente durante il caricamento.

{% highlight python %}
df.set_index('Id', inplace=True)
df.index


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/1244/1*3_Fx6w6QClEYV8Hu6TEF2g.png)</figure>Durante questa operazione, se poniamo *inplace* a *True* andiamo ad aggiornare il DataFrame corrente. Se poniamo *inplace = False* (valore di default) andremo a creare una copia del DataFrame in cui la colonna Id sarò indice senza modificare il DataFrame di partenza.

Questa operazione può essere fatta anche durante il caricamento del file.

{% highlight python %}
df = pd.read_csv(PATH, index_col='Id')


{% endhighlight %}

Vediamo che aspetto ha il nostro DataFrame dopo aver impartito questo comando.

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*JB3Jn5Mror4O6ETJt2ijjg.png)</figure>Ora i dati sono un po’ più puliti!

## Righe e colonne

Come vi siete accorti un DataFrame è una tabella che contiene dati e questa tabella è organizzata in righe e colonne. In *Pandas*, un singola colonna viene chiamata *Serie*. E’ possibile vedere l’elenco delle colonne di un DataFrame con il comando seguente.

{% highlight python %}
df.columns


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/1366/1*R6EWPCNB3oY2_nirL4ZtPg.png)</figure>{% highlight python %}
df['LotArea'].head()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/522/1*_qn-69ByRaMe8E7flm2rQA.png)</figure>{% highlight python %}
type(df['LotArea'])


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/488/1*al2CI9DXApHqFPTrd1KmgA.png)</figure>Notate che una colonna nel nostro DataFrame è di tipo *Series*.

**Rinominiamo le colonne**

E’ molto semplice rinominare le colonne con *Pandas*. Per esempio prendiamo la feature *BedroomAbvGr*, e rinominiamola *Bedroom*.

{% highlight python %}
df.rename(columns={'BedroomAbvGr': 'Bedroom'}, inplace=True)


{% endhighlight %}

Si possono rinominare molte colonne con un solo comando basta utilizzare un dizionario che abbia le due chiavi “old names” e “new names” quando si invoca il metodo *rename*.

**Aggiungiamo una colonna**

Ci potrebbe essere la necessità di aggiungere una colonna al nostro DataFrame. Vediamo come fare.

Cogliamo l’opportunità per mostrare prima come si crea una copia del DataFrame. Aggiungeremo una colonna alla nostra copia appena crata in modo da non cambiare il DataFrame originale.

{% highlight python %}
df_copy = df.copy()
df_copy['Sold'] = 'N'


{% endhighlight %}

Questo è il modo più facile di creare una colonna. Notiamo che abbiamo assegnato il valore **”N”** a tutte le righe della colonna.

Ecco il DataFrame con la nuova colonna.

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*Wvqs89iny8w1LZjNMcHg8Q.png)</figure>**Adding rows**

Ora supponiamo di avere un DataFrame (*df\_to\_append*) che contiene due righe che vogliamo aggiungere a *df\_copy*. Uno dei modi di aggiungere righe alla fine del DataFrame è con il metodo *append*.

{% highlight python %}
data_to_append = {'LotArea': [9500, 15000],
                  'Steet': ['Pave', 'Gravel'],
                  'Neighborhood': ['Downtown', 'Downtown'],
                  'HouseStyle': ['2Story', '1Story'],
                  'YearBuilt': [2021, 2019],
                  'CentralAir': ['Y', 'N'],
                  'Bedroom': [5, 4],
                  'Fireplaces': [1, 0],     
                  'GarageType': ['Attchd', 'Attchd'],  
                  'GarageYrBlt': [2021, 2019],
                  'GarageArea': [300, 250],
                  'PoolArea': [0, 0],
                  'PoolQC': ['G', 'G'],
                  'Fence': ['G', 'G'], 
                  'SalePrice': [250000, 195000],
                  'Sold': ['Y', 'Y']}
df_to_append = pd.DataFrame(data_to_append)
df_to_append

{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*ERQa8iLMeun4AqR2c8zTvw.png)</figure>Ora aggiungiamo le due righe al DataFrame su *df\_copy*.

{% highlight python %}
data_to_append = {'LotArea': [9500, 15000],
df_copy = df_copy.append(df_to_append, ignore_index=True)

{% endhighlight %}

Vediamo cosa abbiamo in *df\_copy*:

{% highlight python %}
df_copy.tail(3)


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*ixAB4LFq6M4DGxh_4r7-Dg.png)</figure>**Rimuoviamo righe e colonne**

Al fine di eliminare righe e colonne da un DataFrame, possiamo usar il metodo *drop*. Immaginiamo di voler cancellare l’ultima riga e le colonna ‘Fence’.

{% highlight python %}
df_copy.drop(labels=1461, axis=0, inplace=True)
df_copy.drop(labels='Fence', axis=1, inplace=True)

{% endhighlight %}

La prima istruzione rimuove l’ultima riga, quella con *Id* = 1461. E’ possibile rimuoovere più di una riga con una sola chiamata fornendo una lista di indici come argomento.

Ponendo axis=0 (valore di default) chiediamo di rimuovere una riga. Se volessimo rimuovere una colonna porremmo axis=1.

## Accedere ai dati con loc e iloc

Uno dei modi più semplici per accedere ai dati è attraverso i metodi *loc* e *iloc*.

*loc* è utilizzato per accedeere alle righe e alle colonne attraverso etichetta (label) e indice oppure basandosi su di un array booleano. Immaginiamo di voler accedere alla riga con indice 1000.

{% highlight python %}
df.loc[1000]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/504/1*9NP2p7Jxc_3jOS2RG2AicA.png)</figure>Il metodo in alto seleziona la riga con indice 1000 e mostra i dati contenuti in questa riga. Possiamo anche selezionare la colonna che vogliamo visualizzare.

{% highlight python %}
df.loc[1000, ['LotArea', 'SalePrice']]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/488/1*HkP4eQj1MEPA45KGFapYeg.png)</figure>Ora vediamo come possiamo applicare una condizione (o filtro) a *loc*. Immaginiamo di voler selezionare le case che hanno un prezzo di almeno $600,000.

{% highlight python %}
df.loc[df['SalePrice'] >= 600000]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*5pI-nxkxeuIL7i84a59b4A.png)</figure>Con una semplice riga di codice abbiamo trovato le 4 case che hanno un valore al di sopra di $600,000.

*iloc* viene utilizzato per selezionare dati basandosi sulla loro posizione espressa attraverso gli indici come numeri interi oppure come array di valori booleani. Per esempio se vogliamo selezionnare il dato contenuto nella prima riga alla prima colonna utilizziamo:

{% highlight python %}
df.iloc[0,0]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/156/1*F3llOmLMIJkYIRWeCwtrxQ.png)</figure>Il valore mostrato è il valore contenuto nella colonna *LotArea* nella riga con *ID* pari a 1. Ricorda che il conteggio degli indici inizia sempre da zero.

Possiamo anche selezionare una intera riga, in questo caso la riga in posizione 10:

{% highlight python %}
df.iloc[10,:]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/464/1*D9LSuZATAoAIdS26ouTCKQ.png)</figure>E possiamo selezionare una intera colonna, per esempio l’ultima:

{% highlight python %}
df.iloc[:,-1]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/812/1*h2PDLZ3BMUavq7dlziuq2g.png)</figure>Possiamo utilizzare le tecniche di slicing viste per le liste.

{% highlight python %}
df.iloc[8:12, 2:5]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/720/1*MByQHdDyG9VMNK8tPyyZJA.png)</figure># Gestire i valori mancanti

In un mondo ideale avremmo dati corretti, accurati e completi. Purtrroppo il mondo ideale non esiste. Un problema particolare sono i dati mancanti. Vediamo come Pandas ci viene in aiuto.

## Cerchiamo valori mancanti

Il primo passo consiste nel trovare i valori che sono mancanti nel nostro DataFrame. Utilizziamo il metodo *isnull*. Questo metodo restituisce un oggetto con la stessa forma del DataFrame originale che contiene i valori True e False per ciascun elemento del DataFrame originale. *True* apparirà se un valore è mancante, False in caso contrario.

{% highlight python %}
df.isnull()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/2000/1*6krVxLO-KlplLSGUldJ-PA.png)</figure>Nota che non è sempre semplice lavorare con i dati restituiti qui in alto. See stai lavorando con un DataFrame molto piccolo va bene ma se hai milioni di righe è meglio aggregare i valori per colonna come nell’esempio seguente:

{% highlight python %}
df.isnull().sum()


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/422/1*1XUaaTXM8fjaGXVvA7fDyQ.png)</figure>Ora è molto meglio! Ci possiamo rendere conto dell’accuratezza dei nostri dati. Molte colonne sono complete e queesto va bene. Il metodo *sum* ha sommato tutti i valori *True* restituiti da *isnull* perché questi sono equivalenti a 1. I valori pari a *False* sono equivalenti a 0.

Possiamo anche calcolare il rapporto tra il numero di valori mancanti e il numero di valori in ciascuna colonna:

{% highlight python %}
df.isnull().sum() / df.shape[0]


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/486/1*FOiFBAZyQsO05KEPtzmkhg.png)</figure>Facciamo un passo avanti e scriviamo un script che calcoli le percentuali dei valori che sono mancanti.

{% highlight python %}
for column in df.columns:
    if df[column].isnull().sum() > 0:
        print(column, ': {:.2%}'.format(df[column].isnull().sum() / df[column].shape[0]))

{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/382/1*EVO89zxch2olG6HsWVHNaw.png)</figure>## Rimuoviamo i valori mancanti

Dopo aver capito quali e quanti sono i valori mancanti dobbiamo decidere cosa fare con questi. Ora vedremo come eliminarli.

Dobbiamo essere molto cauti prima di rimuovere una intera riga o colonna dalla nostra fonte dati perché potremmo danneggiare la nostra analisi.

Iniziamo dalla caratteristica (feature) che si chiama *PoolQC*. Dato che il 99% dei valori sono mancanti la rimuoveremo. Come abbiamo visto in precedenza possiamo eliminare una colonna intera con il metodo *drop*.

Utilizzeremo una copia del DataFrame originale.

{% highlight python %}
df_toremove = df.copy()df_toremove.drop(labels='PoolQC', axis=1, inplace=True)


{% endhighlight %}

Ora poniamo l’attenzione sulla caratteristica *GarageType*. Dato che ci sono il 5% di valori mancanti possiamo semplicemente cancellare le righe con valore nullo utilizzando il metodo *dropna*.

{% highlight python %}
df_toremove.dropna(subset=['GarageType'], axis=0, inplace=True)


{% endhighlight %}

## Compiliamo i valori mancanti

Quando opertiamo con valori mancanti possiamo pensare di “indovinare” il valore che ci sarebbe stato se non fosse stato mancante. Ci sono molte tecniche che ci aiutano in questo, alcune basate sul machine learning. Per ora vediamo cosa *Pandas* mette a nostra disposizione a questo fine.

Diamo una prima occhiata alla caratteristica *Fence*. Abbiamo l’ 80% di valori mancanti. Immaginiamo che sia così perché queste case non hanno la rete (Fence). Basandoci su questo riempiamo tutti i valori mancanti con la stringa *NoFence*.

{% highlight python %}
df_tofill = df.copy()
df_tofill['Fence'].fillna(value='NoFence', inplace=True)


{% endhighlight %}

Controlliamo ora la caratteristica *GarageYrBlt*. In questo esempio vedremo come riempire i valori mancanti con il valore mediano di questa caratteristica.

{% highlight python %}
garage_median = df_tofill['GarageYrBlt'].median()
df_tofill.fillna({'GarageYrBlt': garage_median}, inplace=True)


{% endhighlight %}

Questi esempi hanno il solo scopo educativo.

Ci siamo accorti che *GarageType* e *GarageYrBlt* hanno 81 valori mancanti. Supposiamo che questo sia dovuto al fatto che queste case non hanno garage. Rimuovere delle righe intere in questo caso di certo non è la cosa più intelligente da fare. In effetti quando *GarageType* era mancante, era mancante anche *GarageYrBlt*. Questo mostra l’importanza di saper interpretare i dati con cui si è a contatto.

Abbiamo sempre lavorato con delle copie del DataFrame originale perché non vogliamo rovinare i dati.

# Visualizzare i dati

In questa sezione vedremo come visualizzare i dati con *Pandas*. Vedremo i due tipi di grafico di uso più comune: gli istogrammi e i diagrammi a punti.

## Istogrammi

Gli istogrammi sono molto utili per capire le distribuzioni dei dati. Disegnamo l’istogramma della caratteristica *SalePrice*, dove l’asse **x** contiene insiemi dei valori divisi in intervalli, e l’asse **y** ci mostra quanto frequenti sono quei valori.

{% highlight python %}
df['SalePrice'].plot(kind='hist')


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/812/1*C8hCTPpaKFy_2vJPPiXLaA.png)</figure>## Diagrammi a punti

Con i diagrammi a punti possiamo vedere la relazione tra due variabili mostrando i valori su di un piano cartesiano. Mostriamo una caratteristica sull’asse **x** e l’altra sull’asse **y**.

{% highlight python %}
df.plot(x='SalePrice', y='YearBuilt', kind='scatter')


{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/794/1*E-g_jn1qr8ukvEKSHGaIjQ.png)</figure>Possiamo vedere che c’è una piccola relazione positiva tra il prezzo di vendita e l’anno di costruzione. Gli edifici più nuovi tendenzialmente si vendono ad un prezzo più alto.

Il metodo *plot* supporta molti tipi di grafico: a linee, a barre, ad area, a torta, ecc.

# Salvare su file

In questa ultima sezione vedremo come salvare il DataFrame lavorato su file.

Per salvare il file in formato *csv* utilizziamo il metodo *to\_csv* seguito dal nome del file.

{% highlight python %}
df.to_csv('My_DataFrame.csv')

{% endhighlight %}

Il file salvato può essere trovato nella cartella Files in alto a sinistra nella pagina di Google Colab.

<figure class="wp-block-image">![](https://miro.medium.com/max/658/1*sZYDVA8b9mFOJ7tP6Onmyg.png)</figure>Possiamo anche specificare un percorso in locale sulla nostra macchina:

{% highlight python %}
df.to_csv('C:/Users/username/Documents/My_DataFrame.csv')   

{% endhighlight %}

<figure class="wp-block-image">![](https://miro.medium.com/max/1400/1*lVlFpvldmpluAqkxVrFyfg.jpeg)</figure># Conclusioni

Spero che questa pagina abbia aiutato a capire le potenzialità di questa libreria. Con un po’ di pratica manipolare i dati diventerà naturale.

*Pandas* è molto utilizzato nell’ambito della data science. Di solito viene utilizzata per una analisi prelimininare per capire e “aggiustare” i dati.

Probabilmente ci vorrà un po’ di sforzo per imparare ad utilizzarla ma presto diventa un coltellino svizzero essenziale.
