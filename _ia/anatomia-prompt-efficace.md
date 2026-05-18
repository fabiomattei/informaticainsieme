---
title: 'Anatomia di un Prompt Efficace'
date: '2026-05-17T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

# Anatomia di un Prompt Efficace

Un prompt ben costruito si compone di cinque elementi. Non è necessario usarli tutti ogni volta: un buon punto di partenza è **Compito + Contesto + Formato**.

| Elemento | Domanda | Esempio |
|----------|---------|---------|
| **Ruolo** | Chi deve essere l'AI? | *"Sei un esperto di..."* |
| **Contesto** | Cosa deve sapere prima? | *"Lavoro su un progetto..."* |
| **Compito** | Cosa deve fare? | *"Riassumi / Genera / Confronta..."* |
| **Formato** | Come deve rispondere? | *"Tabella, bullet, 200 parole..."* |
| **Vincoli** | Cosa NON fare? | *"In italiano, no termini tecnici..."* |

# La Formula: R · C · T · F · V in Pratica

Un esempio concreto di prompt costruito con tutti e cinque gli elementi:

> **[RUOLO]** Sei un copywriter esperto di e-commerce. **[CONTESTO]** Devo lanciare uno zaino da viaggio per nomadi digitali, prezzo €120. **[COMPITO]** Scrivi 3 varianti di descrizione prodotto. **[FORMATO]** Ogni variante max 60 parole, con un titolo accattivante. **[VINCOLI]** Tono ironico, niente claim non verificabili.

Stessa AI, prompt diverso = risposta professionale, non generica.

# 3 Tecniche da Tenere nello Zaino

**Zero-Shot** — *Chiedi e basta*

Nessun esempio. Funziona per compiti semplici e ben definiti.

> *"Traduci questo testo in inglese."*

**Few-Shot** — *Mostra 2-3 esempi*

Dai esempi del tipo di output che vuoi. L'AI imita lo stile.

> Input: cane → 🐕  
> Input: gatto → ?

**Chain-of-Thought** — *Ragiona passo passo*

Chiedi all'AI di esplicitare il ragionamento prima della risposta.

> *"Pensa passo passo, poi rispondi."*

# Esempio 1 — Una Email Professionale

| | Prima | Dopo |
|---|---|---|
| **Prompt** | *"Scrivi un'email per chiedere un rinvio."* | *"Sei un assistente professionale. Scrivi un'email al cliente Marco Rossi per posticipare la riunione di lunedì 10 a mercoledì 12. Tono cortese e diretto, max 100 parole, in italiano."* |
| **Risultato** | Risposta vaga, tono indefinito, motivo non chiaro — da riscrivere. | Email pronta da inviare: oggetto, saluto, motivo, alternativa, chiusura. |

# Le 5 Trappole Più Comuni

- **Troppo vago** — *"Aiutami con il marketing."* Senza dettagli l'AI non sa da dove iniziare.
- **Niente contesto** — L'AI non sa per chi né perché.
- **Tutto in un colpo** — 10 richieste in un solo prompt.
- **Nessun formato** — Bullet, tabella, paragrafo? Decidi tu.
- **Fidarsi ciecamente** — Verifica sempre fatti e numeri.

# Ciò che Ogni Prompter Deve Sapere

- **Allucinazioni** — L'AI può inventare con sicurezza fatti, citazioni, link. Verifica sempre.
- **Bias** — Riflette i pregiudizi presenti nei dati di addestramento. Pensiero critico richiesto.
- **Privacy** — Non incollare dati personali, sanitari o aziendali riservati nei prompt.
- **Diritto d'autore** — L'output può essere ispirato a opere protette: attenzione all'uso commerciale.
