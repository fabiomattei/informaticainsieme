---
title: 'C++: le classi'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Dati e comportamenti insieme

Una **classe** è un tipo definito dal programmatore che raggruppa **dati**
(attributi) e **funzioni** (metodi) in un'unica entità. Un **oggetto** è
un'istanza concreta di una classe.

In C++ i membri di una classe possono essere `private` (accessibili solo
dall'interno della classe) o `public` (accessibili dall'esterno).
Per default, tutti i membri di una `class` sono `private`.

---

## Prima classe: attributi e costruttore

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

class Persona {
private:
    string nome;
    int    eta;

public:
    Persona(string n, int e) {
        nome = n;
        eta  = e;
    }

    void presenta() {
        cout << "Mi chiamo " << nome << " e ho " << eta << " anni." << endl;
    }
};

int main() {
    Persona p1("Alice", 30);
    Persona p2("Bob",   25);
    p1.presenta();
    p2.presenta();
    return 0;
}
{% endhighlight %}

Il **costruttore** ha lo stesso nome della classe e nessun tipo di ritorno.
Viene chiamato automaticamente quando si crea un oggetto.

---

## Getter e setter

Per leggere e modificare attributi privati si usano metodi pubblici
chiamati **getter** e **setter**.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

class Rettangolo {
private:
    double base;
    double altezza;

public:
    Rettangolo(double b, double a) : base(b), altezza(a) {}

    double getBase()     { return base; }
    double getAltezza()  { return altezza; }
    void   setBase(double b)    { if (b > 0) base = b; }
    void   setAltezza(double a) { if (a > 0) altezza = a; }

    double area()       { return base * altezza; }
    double perimetro()  { return 2 * (base + altezza); }
};

int main() {
    Rettangolo r(5.0, 3.0);
    cout << "Area: "      << r.area()      << endl;
    cout << "Perimetro: " << r.perimetro() << endl;

    r.setBase(8.0);
    cout << "Nuova area: " << r.area() << endl;

    return 0;
}
{% endhighlight %}

La sintassi `: base(b), altezza(a)` nel costruttore è la **lista di
inizializzazione**: è più efficiente dell'assegnazione nel corpo.

---

## Ereditarietà

Una classe può **ereditare** da un'altra usando `: public NomeClasse`.
La classe derivata eredita tutti i membri pubblici e può aggiungerne di nuovi.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

class Animale {
protected:
    string nome;
public:
    Animale(string n) : nome(n) {}
    void descrivi() {
        cout << "Sono un animale di nome " << nome << endl;
    }
};

class Cane : public Animale {
public:
    Cane(string n) : Animale(n) {}
    void abbaia() {
        cout << nome << " dice: Bau!" << endl;
    }
};

class Gatto : public Animale {
public:
    Gatto(string n) : Animale(n) {}
    void miagola() {
        cout << nome << " dice: Miao!" << endl;
    }
};

int main() {
    Cane  c("Rex");
    Gatto g("Micio");

    c.descrivi();
    c.abbaia();
    g.descrivi();
    g.miagola();

    return 0;
}
{% endhighlight %}

`protected` è simile a `private` ma i membri sono accessibili anche nelle
classi derivate.

---

## Polimorfismo con funzioni virtual

`virtual` permette alle classi derivate di ridefinire un metodo della
classe base. Il metodo corretto viene scelto a **runtime** in base al
tipo reale dell'oggetto.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

class Forma {
public:
    virtual double area() = 0;  // metodo puramente virtuale
    virtual void descrivi() {
        cout << "Area: " << area() << endl;
    }
};

class Cerchio : public Forma {
    double raggio;
public:
    Cerchio(double r) : raggio(r) {}
    double area() override { return 3.14159 * raggio * raggio; }
};

class Quadrato : public Forma {
    double lato;
public:
    Quadrato(double l) : lato(l) {}
    double area() override { return lato * lato; }
};

int main() {
    vector<Forma*> forme = {new Cerchio(5), new Quadrato(4), new Cerchio(3)};
    for (Forma* f : forme) {
        f->descrivi();
    }
    for (Forma* f : forme) delete f;
    return 0;
}
{% endhighlight %}

`= 0` rende il metodo **puramente virtuale**: la classe `Forma` non può
essere istanziata direttamente e ogni classe derivata *deve* implementare
`area()`.

---

## Esercizi

#### Esercizio 5
Crea una classe `Studente` con attributi `nome`, `cognome` e `votoMedio`.
Aggiungi un metodo `isPromosso()` che restituisce `true` se il voto medio
è ≥ 6. Crea tre istanze e testa il metodo.

#### Esercizio 6
Crea una classe `ContoBancario` con attributo `saldo`. Aggiungi i metodi
`deposita(double)`, `preleva(double)` (che non va sotto zero) e
`getSaldo()`. Testa il comportamento.

#### Esercizio 7
Crea una classe base `Veicolo` con attributo `velocitaMax` e un metodo
`virtual descrivi()`. Crea le classi derivate `Auto`, `Moto` e `Camion`
che ridefiniscono `descrivi()`. Usale in un vettore di `Veicolo*`.

#### Esercizio 8
Crea una classe `Punto` con coordinate `x` e `y`. Aggiungi un metodo
`distanzaDa(Punto altro)` che calcoli la distanza euclidea tra due punti.
