---
title: 'Sicurezza AI'
date: '2026-05-18T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

# Rischi di Sicurezza più Comuni

## Sistemi AI Odierni

- Prompt injection e manipolazione degli input
- Data poisoning e compromissione del training
- Data leakage e violazioni della privacy
- Supply chain AI

## Sistemi Agentic AI

- Espansione drastica della superficie di attacco
- Agent hijacking e bypass delle guardrail
- Privilege creep e identità non umane
- Azioni autonome irreversibili
- Emergent behavior e cascading failures
- Perdita di osservabilità e accountability

## Rischi Sistemici e di Governance

- Adozione più rapida delle capacità di governance
- Autonomia che supera policy, compliance e controlli interni
- Difficoltà di integrazione nei framework di risk management tradizionali (ISO, NIST, Zero Trust)

# Zero Trust Applicata all'AI

**Non fidarti mai. Verifica sempre.**

La Zero Trust non "blocca tutto": rende ogni accesso consapevole, controllato e revocabile.

- Il perimetro non basta più: utenti, app, modelli e dati sono distribuiti.
- Ogni richiesta viene valutata nel contesto: identità, dispositivo, rischio, dato.
- La fiducia è temporanea: si concede, si misura, si revoca.

## I Tre Principi

Applicati all'AI generativa, diventano un modo pratico per gestire prompt, dati, plugin, agenti e azioni automatiche.

1. **Verifica esplicita** — *Chi o cosa sta chiedendo?*
   Utente, agente, modello, API e connettori vanno identificati e valutati prima di accedere.

2. **Minimo privilegio** — *Quanto serve davvero?*
   Accessi limitati a dati, strumenti e azioni necessari, per il tempo strettamente utile.

3. **Assumi la violazione** — *E se qualcosa fosse compromesso?*
   Progetta pensando a prompt injection, data leak, tool abusati o output manipolati.

*Fonti: NIST SP 800-207 · Microsoft Zero Trust · OWASP Top 10 for LLM Apps*

# Implementazione

L'obiettivo non è "fidarsi del modello", ma governare il percorso completo: richiesta → dati → ragionamento → strumenti → risposta.

**Pipeline**: Identità → Policy → Dati → Tool → Output → Monitor

1. Inventario AI
2. Dati classificati
3. Identità agenti
4. Guardrail prompt/output
5. Azioni approvabili
6. Log, test e risposta

# Mappatura Zero Trust e Rischi Mitigati

**Principi Zero Trust applicati ai sistemi Agentic AI:**

| Principio Zero Trust | Applicazione Agentic AI |
|---|---|
| Never trust, always verify | Ogni azione dell'agente va autenticata |
| Least privilege | Scope minimo per tool e API |
| Continuous evaluation | Rivalutazione per ogni step decisionale |
| Assume breach | Agente considerato potenzialmente compromesso |
| Strong identity | Identità non umane gestite come umane |
| Policy decision points | Autorizzazione centrale per ogni azione |

**Rischi specifici e meccanismi di mitigazione:**

| Rischio | Meccanismo Zero Trust |
|---|---|
| Agent hijacking | Autenticazione step-based |
| Privilege escalation | Token temporanei |
| Lateral movement | Micro-segmentazione |
| Data exfiltration | Policy-based egress control |
| Shadow agents | Inventory + identity lifecycle |
