---
id: 901
title: 'c++ le stringhe'
date: '2021-12-13T07:41:19+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/?p=901'
permalink: '/?p=901'
---

In C e in C++ non esiste un vero e proprio tipo stringa.  
Una stringa é vista come una sequenza di caratteri  
terminata dal terminatore \\0.  
Una stringa, contenuta in uno spazio in memoria  
(array). La gestione delle stringhe in standard C é facilitata da  
una serie di funzioni contenute in string.h, e stdlib.h.

#### Assegnamento

Ad una variabile di tipo stringa é possibile  
assegnare un valore di una stringa, un carattere  
o un’altra variabile di tipo stringa.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string nome1, nome2, miastringa;
const char a=12;
nome1 = "Mario";
nome2 = nome1;
miastringa = 'n';
miastringa = a;
```

</div>#### Assegnamenti multilinea

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">// Non é possibile andare a capo in un assegnamento a
// costante stringa
string sbagliata= "stringa veramente troppo, //ERRORE!
lunga "";
// ...cosí peró va bene...
string corretta= "stringa veramente troppo, \n "
"lunga ";
```

</div>#### Output di stringhe

Una stringa si puó stampare inserendolo su uno  
stream di output.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string saluto="ciao mondo!!";
cout << saluto  << endl;
```

</div>#### Input di stringhe

E’ possibile utilizzare l’operatore di estrazione &gt;&gt; su cin per raccogliere una stringa inserita dall’utente. Tuttavia, cin considera il carattere spazio (spazio bianco, tab, etc) come terminatore, quindi si può fare l’input di una sola parola.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string nome;
cout << "Scrivi il tuo nome: ";
cin >> nome; // raccoglie l'input dall'utente
cout << "Il tuo nome: " << nome;

// Scrivi il tuo nome:  Mario
// Il tuo nome: Mario
```

</div><div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string nome;
cout << "Scrivi il tuo nome: ";
cin >> nome; // raccoglie l'input dall'utente
cout << "Il tuo nome: " << nome;

// Scrivi il tuo nome:  Mario  Rossi
// Il tuo nome: Mario
```

</div>Si utilizza la funzione getline che legge una linea da un file di testo fino a quando incontra il fine-linea \\n.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string nome;
cout << "Scrivi il tuo nome: ";
getline(cin, nome);
cout << "Il tuo nome: " << nome;

// Scrivi il tuo nome:  Mario  Rossi
// Il tuo nome: Mario  Rossi
```

</div>La funzione getline può prendere due o tre parametri:

```
<pre class="wp-block-preformatted">istream& getline (istream&  is, string& str, char delim);
```

- **istream** oggetto da cui i caratteri sono estratti.
- **str** oggetto di tipo stringa al cui interno verranno conservati i caratteri recuperat.
- **delim** carattere separatore

#### Lunghezza di una stringa

Le stringhe sono normali classi C++ dotati di funzioni membro  
Il primo metodo che vediamo misura la lunghezza di una stringa.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">int i;
string saluto="ciao";
i = saluto.length();  // i assume il valore 4

```

</div>#### Controlliamo se una stringa è vuota

Il metodo membro bool empty() restituisce true se la stringa non contiene alcun carattere e false altrimenti.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string s1 = "";
cin >> s1;
if (s1.empty()) {
    cout << "Lettura fallita!" << endl;
} else {
    cout << "Ho letto: " << s1 << endl;
}
```

</div>#### Concatenazione

Le stringhe possono essere concatenate tramite l’operatore (+)

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string saluto = "ciao";
string chi = "mondo";
string saluti = saluto +", "+chi+"!"
// equivale a 
string saluti = "ciao, mondo!";
```

</div>#### Accesso agli elementi di una stringa

L’accesso agli elementi di una stringa puó avvenire tramite funzione membro at(int i) o tramite operatore \[\]. Il primo carattere é alla posizione zero.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string s="liceo scientifico";
char c;
c = s.at(2);  // c = 'c'
c = s[1];     //c='i'
```

</div>#### Inserimento in una stringa

Una stringa puó essere inserita in un’altra tramite il metodo:  
string insert(int startpos, string s)

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string s = "Liceo scienze applicate";
string s1 = "scientifico ";
s.insert(6, s1);   // s diviene "Liceo scientifico scienze applicate"
```

</div>#### Estrazione di una sottostringa

L’estrazione di una sottostringa viene fatta tramite il metodo  
string &amp; substr(int start, int num)

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string s = "Liceo scientifico scienze applicate";
string s1 = s.substr(6,11);
cout << s1 << endl; //stampa scientifico
```

</div>#### Cancellazione di una sottostringa

La cancellazione di una sottostringa viene fatta tramite il metodo  
string &amp; erase(int start, int num)

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string s = "Mario Nicola Verdi";
s.erase(5,7);
cout << s << endl; //stampa Mario Verdi
```

</div>#### Sostituzione di una sottostringa

Si puó cancellare e sostituire una sottostringa all’interno di una stringa tramite il metodo  
string &amp; replace(int start, int num, string s)

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string s = "Mario Rossi";
s.replace(0,5,"Marco");
cout << s << endl; //stampa "Marco Rossi"
```

</div>#### Ricerca di una sottostringa

Si puó cercare una sottostringa all’interno di una stringa tramite il metodo  
int find(string s, int startSearch)

Il metodo ricerca a partire dal carattere occupante la posizione ad indice startSearch e restituisce il numero intero indicante la posizione in cui la sottostringa inizia.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="c_cpp" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">string s= "Mario Rossi";
int a = s.find("R",0);
cout << a << endl; //stampa 6
```

</div>