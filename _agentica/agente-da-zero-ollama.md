---
title: "Un agente da zero: l'esempio con Ollama"
date: '2026-08-14T09:30:00+02:00'
author: Fabio Mattei
layout: page
---

Le pagine precedenti di questa sezione spiegano i concetti dell'informatica agentica in astratto. Qui invece costruiamo un esempio concreto, **senza framework**, per vedere con i propri occhi cosa succede davvero dentro il ciclo di un agente. L'obiettivo è un agente a riga di comando che risponde a domande su un repository di codice — ad esempio "dove viene gestita l'autenticazione?" — esplorando i file da solo.

Costruirlo "a mano" invece di usare un framework agentico già pronto è scelta deliberata: un framework nasconde proprio i dettagli che qui vogliamo vedere.

## L'idea di base

L'agente ha due ingredienti essenziali: un modello linguistico capace di **tool use** (vedi [Tool use e function calling]({{ site.baseurl }}{% link _agentica/tool-use-function-calling.md %}.html)) e un piccolo insieme di strumenti che gli permettono di guardare dentro al progetto:

* `list_files(directory)` — elenca i file presenti in una cartella
* `read_file(path)` — legge il contenuto di un file
* `search_code(query)` — cerca una stringa nei file, come un `grep`

Il modello non "vede" il filesystem: ogni volta che vuole sapere qualcosa, deve chiedere esplicitamente uno di questi strumenti, e il codice Python esegue davvero l'operazione e gli restituisce il risultato.

## Il cuore dell'esercizio: il loop agentico

Tutto il comportamento "intelligente" dell'agente nasce da un ciclo molto semplice, ripetuto finché il modello non ha una risposta finale:

{% highlight python %}
messages = [{"role": "user", "content": domanda}]

while True:
    response = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=1024,
        tools=[list_files_tool, read_file_tool],
        messages=messages
    )

    if response.stop_reason != "tool_use":
        break  # l'agente ha finito, ha dato la risposta finale

    # esegui il tool richiesto e rimanda il risultato
    messages.append({"role": "assistant", "content": response.content})
    tool_result = esegui_tool(response.content)
    messages.append({"role": "user", "content": tool_result})
{% endhighlight %}

Stampando ogni chiamata a tool si vede chiaramente la strategia che il modello sceglie da solo: in genere parte con `list_files` per orientarsi, poi usa `read_file` sui file candidati, magari due o tre letture prima di rispondere. Nessuna di queste decisioni è scritta a mano nel codice: è il modello a deciderle, passo per passo, osservando i risultati precedenti.

## La stessa architettura, in locale con Ollama

Il ciclo appena visto funziona anche senza un servizio esterno a pagamento. **Ollama** supporta il tool calling nativo dalla versione 0.3 in poi, con modelli come `llama3.1`, `qwen2.5` o `mistral-nemo`. La logica resta identica; cambiano solo il client usato per parlare col modello e il formato dei messaggi (Ollama segue una convenzione simile a quella di OpenAI).

Cosa cambia, in pratica, rispetto alla versione con Claude:

* non serve nessuna chiave API: il modello gira interamente sul proprio computer
* il modello va scaricato in anticipo, ad esempio con `ollama pull qwen2.5:7b`
* il formato del risultato dei tool è leggermente diverso da quello dell'API Anthropic
* i modelli più piccoli sono meno affidabili nel decidere quando e come usare gli strumenti: aspettarsi più errori nei parametri o cicli meno "puliti" rispetto a un modello frontier

Ecco il codice completo, `agent_ollama.py`: stessa architettura della versione con Claude (`ProjectTools`, loop, sandboxing) ma basata su `ollama.chat()`:

{% highlight python %}
"""
Stessa architettura di agent.py, ma con un modello locale via Ollama
invece dell'API di Anthropic.

Prerequisiti:
    1. Ollama installato e in esecuzione (https://ollama.com)
    2. Un modello che supporta il tool calling, es.:
         ollama pull qwen2.5:7b
       (alternative valide: llama3.1:8b, mistral-nemo)
    3. pip install ollama

Uso:
    python agent_ollama.py /percorso/al/progetto "dove viene gestita l'autenticazione?"

Nota sull'affidabilita': i modelli locali piccoli (7-8B) sono MOLTO meno
affidabili di Claude nel decidere quando e come usare i tool. Aspettati:
- tool call malformate o con argomenti sbagliati
- il modello che "inventa" una risposta senza usare i tool
- loop che non convergono
Questo e' un buon modo per capire concretamente perche' la qualita' del
modello sottostante conta quanto il design del loop.
"""

import os
import sys
import subprocess
from pathlib import Path

import ollama

MODEL = "qwen2.5:7b"   # cambia qui se usi un altro modello scaricato
MAX_ITERATIONS = 10
MAX_FILE_CHARS = 8000


# ---------------------------------------------------------------------------
# 1. DEFINIZIONE DEI TOOL
#    Ollama usa lo stesso formato "OpenAI-style" per gli schema dei tool.
# ---------------------------------------------------------------------------

TOOLS_SCHEMA = [
    {
        "type": "function",
        "function": {
            "name": "list_files",
            "description": (
                "Elenca i file e le sottocartelle in una directory del progetto. "
                "Usalo per orientarti nella struttura prima di leggere file specifici."
            ),
            "parameters": {
                "type": "object",
                "properties": {
                    "directory": {
                        "type": "string",
                        "description": "Percorso relativo della directory da esplorare, es. '.' o 'src'",
                    }
                },
                "required": ["directory"],
            },
        },
    },
    {
        "type": "function",
        "function": {
            "name": "read_file",
            "description": "Legge il contenuto di un singolo file di testo del progetto.",
            "parameters": {
                "type": "object",
                "properties": {
                    "path": {
                        "type": "string",
                        "description": "Percorso relativo del file da leggere, es. 'src/auth.py'",
                    }
                },
                "required": ["path"],
            },
        },
    },
    {
        "type": "function",
        "function": {
            "name": "search_code",
            "description": (
                "Cerca una stringa o un pattern in tutti i file del progetto (come grep -r). "
                "Molto piu' efficiente di leggere file a caso quando cerchi dove si trova qualcosa."
            ),
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "Testo o pattern da cercare, es. 'def authenticate'",
                    }
                },
                "required": ["query"],
            },
        },
    },
]


class ProjectTools:
    """Identica alla versione con Claude: implementazione sandboxata dei tool."""

    def __init__(self, root_dir: str):
        self.root = Path(root_dir).resolve()
        if not self.root.exists():
            raise ValueError(f"La directory {self.root} non esiste")

    def _safe_path(self, relative_path: str) -> Path:
        candidate = (self.root / relative_path).resolve()
        if not str(candidate).startswith(str(self.root)):
            raise ValueError("Percorso fuori dalla directory del progetto: non consentito")
        return candidate

    def list_files(self, directory: str) -> str:
        target = self._safe_path(directory)
        if not target.exists():
            return f"Errore: la directory '{directory}' non esiste"

        ignore = {".git", "node_modules", "__pycache__", ".venv", "venv"}
        lines = []
        for item in sorted(target.iterdir()):
            if item.name in ignore:
                continue
            marker = "/" if item.is_dir() else ""
            lines.append(f"{item.name}{marker}")

        return "\n".join(lines) if lines else "(directory vuota)"

    def read_file(self, path: str) -> str:
        target = self._safe_path(path)
        if not target.exists():
            return f"Errore: il file '{path}' non esiste"
        if not target.is_file():
            return f"Errore: '{path}' e' una directory, non un file"

        try:
            content = target.read_text(encoding="utf-8", errors="replace")
        except Exception as e:
            return f"Errore nella lettura di '{path}': {e}"

        if len(content) > MAX_FILE_CHARS:
            content = content[:MAX_FILE_CHARS] + "\n\n[... file troncato, troppo lungo ...]"

        return content

    def search_code(self, query: str) -> str:
        try:
            result = subprocess.run(
                ["grep", "-rn", "--include=*.*", query, str(self.root)],
                capture_output=True,
                text=True,
                timeout=10,
            )
        except FileNotFoundError:
            return "Errore: 'grep' non disponibile su questo sistema"

        if not result.stdout:
            return f"Nessun risultato per '{query}'"

        lines = result.stdout.strip().split("\n")[:40]
        relative_lines = [line.replace(str(self.root) + "/", "") for line in lines]
        return "\n".join(relative_lines)


# ---------------------------------------------------------------------------
# 2. IL LOOP AGENTICO
#    Stessa logica della versione Anthropic. Cambia solo come si legge
#    la risposta e come si formattano i messaggi.
# ---------------------------------------------------------------------------

def run_agent(question: str, project_dir: str) -> str:
    tools = ProjectTools(project_dir)

    system_prompt = (
        "Sei un assistente che esplora un repository di codice per rispondere a domande. "
        "Usa i tool a disposizione: di solito conviene iniziare con list_files o search_code "
        "prima di leggere file specifici con read_file. "
        "Rispondi in modo conciso e cita i file rilevanti. "
        "IMPORTANTE: se non hai bisogno di altri tool, rispondi in testo normale senza chiamarne."
    )

    messages = [
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": question},
    ]

    for iteration in range(1, MAX_ITERATIONS + 1):
        print(f"\n--- Iterazione {iteration} ---")

        response = ollama.chat(
            model=MODEL,
            messages=messages,
            tools=TOOLS_SCHEMA,
        )

        message = response["message"]
        tool_calls = message.get("tool_calls")

        # Se il modello non ha chiesto tool, ha finito: risposta finale
        if not tool_calls:
            return message.get("content", "(risposta vuota)")

        # Aggiungiamo il messaggio dell'assistente (con le tool_calls) alla storia
        messages.append(message)

        for call in tool_calls:
            name = call["function"]["name"]
            args = call["function"]["arguments"]  # ollama la restituisce gia' come dict

            print(f"  -> tool: {name}({args})")

            if name == "list_files":
                output = tools.list_files(args.get("directory", "."))
            elif name == "read_file":
                output = tools.read_file(args.get("path", ""))
            elif name == "search_code":
                output = tools.search_code(args.get("query", ""))
            else:
                output = f"Tool sconosciuto: {name}"

            # Ollama vuole il risultato come messaggio di ruolo "tool"
            messages.append({
                "role": "tool",
                "content": output,
            })

    return "Limite di iterazioni raggiunto senza una risposta definitiva. Prova a riformulare la domanda."


# ---------------------------------------------------------------------------
# 3. ENTRY POINT
# ---------------------------------------------------------------------------

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Uso: python agent_ollama.py <percorso_progetto> <domanda>")
        sys.exit(1)

    project_path = sys.argv[1]
    user_question = " ".join(sys.argv[2:])

    try:
        answer = run_agent(user_question, project_path)
    except Exception as e:
        print(f"\nErrore: {e}")
        print("Controlla che Ollama sia in esecuzione (comando: 'ollama serve') "
              f"e che il modello '{MODEL}' sia scaricato (comando: 'ollama pull {MODEL}').")
        sys.exit(1)

    print("\n=== Risposta finale ===")
    print(answer)
{% endhighlight %}

Due dettagli del codice valgono una nota a parte, perché non sono ovvi a prima vista. Il primo è `_safe_path`: ogni tool passa da qui prima di toccare il filesystem, e il controllo impedisce all'agente di "uscire" dalla cartella del progetto (ad esempio con un percorso tipo `../../etc/passwd`) — un esempio minimo, ma concreto, di *sandboxing*. Il secondo è il troncamento in `read_file` (`MAX_FILE_CHARS`) e in `search_code` (le prime 40 righe): senza questi limiti, un file enorme o una ricerca troppo generica riempirebbero la conversazione di testo inutile, facendo lievitare inutilmente il numero di token inviati al modello a ogni iterazione successiva.

Per farlo partire:

{% highlight bash %}
# 1. Installa Ollama: https://ollama.com
# 2. Scarica un modello che supporta il tool calling
ollama pull qwen2.5:7b
# 3. Installa la libreria python
pip install ollama

python agent_ollama.py . "quali tool ha a disposizione questo agente?"
{% endhighlight %}

Nell'output, ogni riga `-> tool: ...` mostra una decisione reale presa dal modello, non uno script prefissato: è l'agente stesso a decidere quando usare `search_code` invece di leggere file a caso, e quando ha raccolto abbastanza contesto per rispondere.

## Cosa si nota confrontando Claude e un modello locale

Lanciando la stessa identica domanda su entrambe le versioni — quella con l'API di Claude e quella con Ollama — e confrontando il numero di iterazioni nel log, emerge una cosa non solo teorica: con modelli locali piccoli (7-8 miliardi di parametri) il ciclo spesso non converge in modo pulito come con un modello frontier. Si osservano tool call con argomenti sbagliati, il modello che risponde a caso senza usare gli strumenti quando servirebbero, oppure che richiama lo stesso tool ripetutamente senza fare progressi.

Non è un bug nel codice dell'agente: è la differenza reale di affidabilità tra un modello frontier e uno locale nel **decidere quando e come agire**, che è esattamente la parte più difficile del comportamento agentico — non l'esecuzione del tool in sé, ma la scelta di quale usare e quando fermarsi. È anche il motivo per cui il limite `MAX_ITERATIONS` non è un dettaglio implementativo secondario: senza quel tetto di sicurezza, un agente confuso potrebbe chiamare tool all'infinito, con un costo in tempo (e, con un servizio a pagamento, anche in denaro) senza mai risolvere il compito.

Qualche punto pratico da tenere presente prima di provarlo:

* **nessun costo, nessuna chiave API, tutto in locale** — ma serve una GPU decente (o parecchia pazienza) per modelli oltre i 7-8 miliardi di parametri;
* **modelli consigliati per il tool calling**: `qwen2.5:7b` o superiore, `llama3.1:8b`, `mistral-nemo`; modelli sotto i 7B reggono raramente il tool calling in modo affidabile;
* **un buon esercizio di confronto** è eseguire la stessa domanda sulla versione con Claude e su quella con Ollama, guardando sia il numero di iterazioni sia la qualità delle decisioni prese — è il modo più diretto per toccare con mano quanto la qualità del modello sottostante conta, a parità di *harness*.

## Due fasi da distinguere: loop ed esecuzione dei test

Vale la pena chiarire due fasi che si intrecciano spesso in questi esempi.

La **fase di loop** è la sequenza ripetuta di quattro passi vista sopra: invio della conversazione al modello, decisione del modello (tool o risposta finale), esecuzione reale del tool da parte del codice — mai del modello stesso — e ritorno del risultato come nuovo messaggio, fino a quando il modello non chiede più strumenti oppure si raggiunge il limite di iterazioni.

La **fase di test** è un passo ulteriore, tipico di un agente che non si limita a rispondere ma modifica anche del codice: dopo una modifica, l'agente non si ferma lì, ma esegue davvero i test automatici (ad esempio con `pytest`) come se fosse un tool aggiuntivo, e legge l'output reale per capire se la modifica ha funzionato. Un `run_tests()` di questo tipo, unito a un tool `write_file`, trasforma l'agente da "esploratore in sola lettura" a un vero ciclo *modifica → verifica → correggi*, tipico degli agenti di programmazione.

## Come estenderlo ulteriormente

L'agente presentato qui è volutamente minimale: legge, ma non scrive nulla. Alcuni passi naturali per portarlo oltre:

1. **Aggiungere un tool che scrive file** (`write_file`) e far sì che l'agente proponga una modifica e poi la verifichi rileggendo il file appena scritto.
2. **Aggiungere un tool `run_tests`** che esegue `pytest` e ne restituisce l'output: a quel punto si è ricostruito il ciclo completo "modifica → verifica" di un vero agente di programmazione.
3. **Registrare il costo**: ogni risposta del modello riporta i token effettivamente usati. Su un ciclo di 10 iterazioni con file grandi i costi salgono in fretta — un problema molto concreto in produzione, non solo un dettaglio contabile.
4. **Provare a "rompere" l'agente di proposito**: chiedergli qualcosa su un file che non esiste, o porre una domanda ambigua. Osservare come si comporta senza guardrail è istruttivo tanto quanto vederlo funzionare bene.

## Che rapporto ha tutto questo con il machine learning?

È una distinzione importante da tenere chiara, perché è facile confondere i piani. Il codice mostrato in questa pagina — la classe `ProjectTools`, il ciclo `while`, la gestione dei file — è **ingegneria del software tradizionale**: if/else, chiamate a funzioni, gestione di percorsi. Nessun training, nessuna discesa del gradiente, nessun dataset.

Il **machine learning** sta invece dentro al modello stesso. Sia Claude sia `qwen2.5` sono stati addestrati con enormi quantità di testo e codice, aggiustando miliardi di parametri durante il training. La capacità di decidere quale tool chiamare, con quali argomenti, e quando fermarsi non è scritta a mano da nessuno: è un comportamento **emerso** dal training, non una regola programmata esplicitamente. Quando il modello "capisce" che per rispondere a una domanda sull'autenticazione conviene prima fare `search_code` invece di leggere file a caso, quella è un'inferenza appresa.

La linea di demarcazione pratica è questa:

* **training** — come il modello ha imparato a ragionare e generare testo: avviene una volta sola, offline, e richiede risorse enormi che nessuno dei due esempi di questa pagina replica
* **inferenza** — ogni singola chiamata `ollama.chat(...)` (o `client.messages.create(...)`) nel ciclo: il modello già addestrato produce un output dato un input, senza imparare nulla di nuovo in quel momento
* **ingegneria agentica** — tutto il resto: il loop, i tool, il sandboxing, i limiti di sicurezza. È adiacente al machine learning ma è ingegneria del software classica, applicata attorno a un modello ML

Un modo semplice per ricordarlo: l'agente costruito in questa pagina è un'**imbracatura** (in inglese, *harness*) attorno a un modello che è machine learning puro — ma l'imbracatura in sé non lo è.
