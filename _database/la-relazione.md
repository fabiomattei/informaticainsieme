---
title: 'La relazione'
date: '2026-08-17T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

![Una relazione 1:N tra le tabelle scuole e studenti, realizzata con una chiave esterna](/images/database/la-relazione/la-relazione.svg){:class="aside-image"}

Una **relazione**, in generale, è un legame tra due cose. In [Il modello entità relazione]({{ site.baseurl }}{% link _database/modello-entita-relazione.md %}.html) abbiamo visto le relazioni a livello concettuale: rombi che collegano entità su un diagramma, con una cardinalità che ne descrive la forma (1:1, 1:N, N:M). Quel diagramma però è solo un disegno: non è ancora un database funzionante.

Questa pagina fa il passo successivo e risponde a una domanda molto concreta: una volta che il diagramma è tradotto in tabelle, **come si realizza fisicamente il legame tra una riga di una tabella e una riga di un'altra**? La risposta, in un database relazionale, è sempre la stessa: tramite le chiavi.

### Chiave primaria e chiave esterna

Ogni relazione tra tabelle si costruisce con due ingredienti:

* **Chiave primaria (PRIMARY KEY)** — la colonna (o l'insieme di colonne) che identifica in modo univoco ogni riga *all'interno della propria tabella*. Non può contenere duplicati né valori NULL.
* **Chiave esterna (FOREIGN KEY)** — una colonna che non identifica righe della propria tabella, ma **contiene il valore della chiave primaria di un'altra tabella**. È lei il vero e proprio "cavo" che collega le due tabelle.

Nell'immagine a lato, la tabella `studenti` non ripete il nome o l'indirizzo della scuola: si limita a memorizzare, per ogni studente, l'`id` della scuola che frequenta. Per sapere tutto il resto (nome, indirizzo...) basta seguire quel numero fino alla tabella `scuole`. Questo è esattamente il meccanismo che evita la ridondanza di cui si parla in [Le forme normali]({{ site.baseurl }}{% link _database/forme-normali.md %}.html).

In SQL la chiave esterna si dichiara con la clausola FOREIGN KEY (sintassi completa in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html#create-table)):

{% highlight sql %}
CREATE TABLE scuole (
id INTEGER PRIMARY KEY AUTOINCREMENT,
nome TEXT
);

CREATE TABLE studenti (
id INTEGER PRIMARY KEY AUTOINCREMENT,
nome TEXT,
scuola_id INTEGER,
FOREIGN KEY (scuola_id) REFERENCES scuole(id)
);
{% endhighlight %}

Vediamo ora come questo stesso meccanismo — una chiave esterna che punta a una chiave primaria — cambia leggermente forma a seconda del tipo di relazione che deve rappresentare.

### Relazione 1:N (uno a molti)

È la relazione più comune: un'istanza del lato "1" può essere collegata a molte istanze del lato "N", ma ogni istanza del lato "N" è collegata a una sola istanza del lato "1". Una scuola ha molti studenti, ma ogni studente frequenta una sola scuola.

**Regola pratica**: la chiave esterna va sempre inserita nella tabella dal lato "N" (quella con cardinalità massima N). Colloca la FK dalla parte sbagliata e il modello non riesce più a rappresentare correttamente la realtà: una singola colonna può contenere un solo valore alla volta, quindi solo il lato "molti" può "scegliere" a quale singola istanza dell'altro lato riferirsi.

{% highlight sql %}
INSERT INTO scuole (nome) VALUES ('Patini Liberatore'), ('De Panfilis');

INSERT INTO studenti (nome, scuola_id) VALUES
('Mario Rossi', 1),
('Enrichetta Lambertini', 1),
('Luca Bianchi', 2);
{% endhighlight %}

Per ricomporre l'informazione — vedere ogni studente insieme al nome della sua scuola — si usa una JOIN, spiegata in dettaglio in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html):

{% highlight sql %}
SELECT studenti.nome, scuole.nome AS scuola
FROM studenti
JOIN scuole ON studenti.scuola_id = scuole.id;
{% endhighlight %}

| nome | scuola |
|---|---|
| Mario Rossi | Patini Liberatore |
| Enrichetta Lambertini | Patini Liberatore |
| Luca Bianchi | De Panfilis |

### Relazione 1:1 (uno a uno)

Ogni istanza di una tabella è collegata ad al massimo un'istanza dell'altra, e viceversa. Esempio classico: una persona ha un solo passaporto, un passaporto appartiene a una sola persona.

**Regola pratica**: si usa la stessa identica tecnica della relazione 1:N — una FOREIGN KEY in una delle due tabelle — ma si aggiunge il vincolo UNIQUE sulla colonna della chiave esterna. Senza UNIQUE, nulla impedirebbe di collegare due passaporti diversi alla stessa persona, trasformando di fatto la relazione in una 1:N.

{% highlight sql %}
CREATE TABLE persone (
id INTEGER PRIMARY KEY AUTOINCREMENT,
nome TEXT,
cognome TEXT
);

CREATE TABLE passaporti (
numero TEXT PRIMARY KEY,
scadenza TEXT,
persona_id INTEGER UNIQUE,
FOREIGN KEY (persona_id) REFERENCES persone(id)
);
{% endhighlight %}

Il vincolo UNIQUE su `persona_id` impedisce che lo stesso `persona_id` compaia due volte nella tabella `passaporti`: al massimo un passaporto per persona.

### Relazione N:M (molti a molti)

Un'istanza della prima tabella può essere collegata a molte istanze della seconda, e viceversa. Uno studente recita in più film, un film ha più attori.

**Regola pratica**: qui una singola colonna FOREIGN KEY non basta più, perché entrambi i lati possono collegarsi a più righe dell'altro lato. Serve una terza tabella, detta **tabella di collegamento** (o *tabella ponte*, o *tabella associativa*), che contiene **due chiavi esterne**, una per ciascuna delle tabelle da collegare. Ogni riga della tabella di collegamento rappresenta una singola coppia collegata. Spesso la chiave primaria della tabella di collegamento è la coppia stessa delle due chiavi esterne.

Questo è esattamente lo schema ATTORI / RECITA / FILM usato negli esercizi di [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html):

{% highlight sql %}
CREATE TABLE attori (
id INTEGER PRIMARY KEY AUTOINCREMENT,
nome TEXT
);

CREATE TABLE film (
id INTEGER PRIMARY KEY AUTOINCREMENT,
titolo TEXT
);

CREATE TABLE recita (
id_attore INTEGER,
id_film INTEGER,
PRIMARY KEY (id_attore, id_film),
FOREIGN KEY (id_attore) REFERENCES attori(id),
FOREIGN KEY (id_film) REFERENCES film(id)
);
{% endhighlight %}

{% highlight sql %}
INSERT INTO attori (nome) VALUES ('Terence Hill'), ('Bud Spencer');
INSERT INTO film (titolo) VALUES ('Lo chiamavano Trinità'), ('...più forte ragazzi!');

INSERT INTO recita (id_attore, id_film) VALUES
(1, 1),
(2, 1),
(2, 2);
{% endhighlight %}

Terence Hill recita in un solo film di questo elenco, mentre Bud Spencer recita in entrambi: nessuna delle due colonne, da sola, potrebbe contenere questa informazione. È proprio grazie alla tabella `recita` — con una riga per ogni coppia attore-film — che la relazione N:M diventa rappresentabile:

{% highlight sql %}
SELECT attori.nome, film.titolo
FROM recita
JOIN attori ON recita.id_attore = attori.id
JOIN film ON recita.id_film = film.id;
{% endhighlight %}

| nome | titolo |
|---|---|
| Terence Hill | Lo chiamavano Trinità |
| Bud Spencer | Lo chiamavano Trinità |
| Bud Spencer | ...più forte ragazzi! |

### Riepilogo

| Tipo di relazione | Dove va la chiave esterna | Vincolo aggiuntivo necessario |
|---|---|---|
| 1:1 | in una delle due tabelle (a scelta) | UNIQUE sulla colonna della FK |
| 1:N | nella tabella dal lato "N" | nessuno |
| N:M | in una tabella di collegamento dedicata, con due FK | la chiave primaria della tabella di collegamento è di solito la coppia delle due FK |

In tutti e tre i casi il principio è lo stesso: **una chiave esterna è un valore che, invece di descrivere qualcosa, punta a un'altra riga**. È questo semplice meccanismo — non qualcosa di più complesso — che permette a un database relazionale di rappresentare legami tra i dati senza doverli ripetere ovunque servano.

### Esercizi

**Esercizio 1 — Relazione 1:N**

Un negozio vuole registrare i propri ordini. Ogni ordine è effettuato da un cliente; un cliente può effettuare più ordini.

1. Scrivi i comandi CREATE TABLE per `clienti` e `ordini`, individuando in quale tabella va inserita la chiave esterna e perché.
2. Inserisci 2 clienti e 3 ordini, distribuendoli in modo che almeno un cliente abbia più di un ordine.
3. Scrivi una query che visualizzi, per ogni ordine, il nome del cliente che lo ha effettuato.

**Esercizio 2 — Relazione 1:1**

Un'azienda vuole associare a ciascun dipendente un badge identificativo. Ogni dipendente ha al massimo un badge, e ogni badge appartiene a un solo dipendente.

1. Scrivi i comandi CREATE TABLE per `dipendenti` e `badge`, con il vincolo che impedisce a un dipendente di avere più di un badge.
2. Inserisci 3 dipendenti e i relativi badge.
3. Prova a inserire (a parole, senza eseguirlo) un secondo badge per un dipendente che ne ha già uno: quale errore restituirebbe il database, e perché?

**Esercizio 3 — Relazione N:M**

Si vuole gestire l'iscrizione degli studenti ai corsi extra-scolastici. Uno studente può iscriversi a più corsi, un corso può avere più studenti iscritti.

1. Progetta le tre tabelle necessarie (comprese le chiavi primarie ed esterne).
2. Scrivi i comandi CREATE TABLE completi.
3. Inserisci 3 studenti, 2 corsi e alcune iscrizioni, in modo che almeno uno studente sia iscritto a entrambi i corsi.
4. Scrivi una query che visualizzi, per ciascun corso, l'elenco degli studenti iscritti.
5. Scrivi una query che conti quanti studenti sono iscritti a ciascun corso (usa GROUP BY).

**Esercizio 4 — Riconoscere il tipo di relazione**

Per ciascuna delle seguenti situazioni, indica se la relazione è 1:1, 1:N o N:M, motiva la risposta e descrivi come la implementeresti con le chiavi:

1. Un libro e la sua casa editrice (una casa editrice pubblica molti libri, un libro ha una sola casa editrice).
2. Una nazione e la sua capitale (una nazione ha una sola capitale, una città è capitale di al massimo una nazione).
3. Un attore e i film in cui recita.
4. Un cittadino italiano e il suo codice fiscale.
