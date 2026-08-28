---
title: 'Ruby on Rails: il routing'
date: '2026-08-27T08:02:00+02:00'
author: Fabio Mattei
layout: page
---

## A cosa serve il routing

Quando il browser richiede un indirizzo, ad esempio
`http://localhost:3000/articoli`, Rails deve capire **quale controller e
quale azione** devono occuparsi di rispondere. Questo compito è affidato al
file **config/routes.rb**, dove si definiscono tutte le rotte
dell'applicazione.

## Rotte semplici

Una rotta semplice collega un metodo HTTP e un percorso ad una coppia
controller/azione:

{% highlight ruby %}
# config/routes.rb
Rails.application.routes.draw do
  get "benvenuto", to: "pagine#home"
end
{% endhighlight %}

In questo esempio, quando il browser richiede `/benvenuto` con una richiesta
di tipo `GET`, Rails chiama l'azione `home` del controller `PagineController`.

## La rotta principale (root)

Si può definire quale pagina mostrare quando si visita la radice del sito
(`http://localhost:3000/`) usando `root`:

{% highlight ruby %}
Rails.application.routes.draw do
  root "pagine#home"
end
{% endhighlight %}

## Rotte per una risorsa: resources

Nella maggior parte delle applicazioni web esiste il bisogno di gestire una
"risorsa" (ad esempio degli articoli) con le classiche operazioni CRUD:
elenco, dettaglio, creazione, modifica, cancellazione. Scrivere una rotta per
ognuna di queste operazioni sarebbe ripetitivo, per questo Rails offre la
scorciatoia `resources`:

{% highlight ruby %}
Rails.application.routes.draw do
  resources :articoli
end
{% endhighlight %}

Questa singola riga genera automaticamente **sette rotte**:

| Metodo HTTP | Percorso              | Azione   | Uso                          |
|:-----------:|------------------------|:--------:|-------------------------------|
| GET         | /articoli              | index    | elenco degli articoli         |
| GET         | /articoli/new          | new      | form per crearne uno nuovo    |
| POST        | /articoli               | create   | salva il nuovo articolo       |
| GET         | /articoli/:id          | show     | mostra un singolo articolo    |
| GET         | /articoli/:id/edit     | edit     | form per modificarlo          |
| PATCH/PUT   | /articoli/:id          | update   | salva le modifiche            |
| DELETE      | /articoli/:id          | destroy  | elimina l'articolo            |

## Vedere tutte le rotte

Per controllare quali rotte sono attive in un progetto, in qualunque momento,
si può usare il comando:

{% highlight bash %}
bin/rails routes
{% endhighlight %}

## Path e URL helper

Ogni rotta genera automaticamente dei metodi di comodo (helper) utilizzabili
nel codice al posto di scrivere gli indirizzi a mano, ad esempio
`articoli_path` restituisce `/articoli` e `articolo_path(3)` restituisce
`/articoli/3`. Questi helper verranno usati spesso nelle viste, come vedremo
più avanti.
