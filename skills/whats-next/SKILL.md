---
name: whats-next
description: Usare quando l'utente descrive ciò che ha già completato e chiede quale sia il prossimo passaggio nel flusso delle skill di sviluppo.
---

# Indicare il prossimo passaggio

## Scopo

Aiutare l'utente a orientarsi nel flusso delle skill del progetto. La skill interpreta ciò che è stato completato, individua il punto raggiunto, segnala eventuali prerequisiti mancanti e propone una sola prossima skill da usare.

La skill è esclusivamente informativa:

- non crea o modifica file;
- non modifica codice, test, issue o documenti;
- non esegue automaticamente la skill successiva;
- non crea commit e non esegue operazioni GitHub o Git remote.

## Quando usarla

- L'utente dice cosa ha fatto e chiede quale sia il prossimo punto del flusso.
- L'utente non sa quale skill usare tra quelle disponibili.
- Il lavoro è stato interrotto e occorre riprendere dal punto corretto.
- Occorre capire se un passaggio è completato o se manca un artefatto.

## Input

Accettare una descrizione libera, per esempio:

```text
Ho creato l'intent e ora vorrei sapere cosa fare.
```

oppure:

```text
Ho implementato la issue #12, i test passano e ho scritto il recap. Qual è il prossimo passaggio?
```

Usare anche eventuali percorsi, riferimenti a issue, branch, commit o spec forniti dall'utente. Se la descrizione è troppo ambigua per distinguere due passaggi consecutivi, porre una sola domanda mirata invece di scegliere silenziosamente.

## Flusso di riferimento

Usare questa sequenza come baseline:

```text
capture-intent
      ↓
intent-to-spec
      ↓
spec-to-issue
      ↓
implement-issue
      ↓
review-implementation (raccomandata ma non obbligatoria)
      ↓
close-issue
      ↓
branch-to-docs
      ↓
validate-traceability (raccomandata ma non obbligatoria)
```

Interpretare il flusso così:

| Punto | Evidenza principale | Prossimo passaggio tipico |
|---|---|---|
| Intent raccolto | `docs/intents/intent-{specname}.md` | `intent-to-spec` |
| Spec prodotta | `docs/specs/spec-{specname}.md` | `spec-to-issue` |
| Proposte di issue prodotte | `docs/issues/issue-{specname}-{NN}-{slug}.md` per ogni unità e riferimenti alle issue | Pubblicazione manuale su GitHub, poi `implement-issue` per la prima issue non implementata |
| Implementazione terminata | codice, test e `dev/implementation/{specname}/{issue}-implementation-recap.md` | `review-implementation` |
| Review completata | report di review senza blocchi | `close-issue` |
| Recap e riferimenti di commit disponibili | uno o più implementation recap | `close-issue` |
| Sessione o branch terminato | branch oppure intervallo `commit-start` / `commit-end` | `branch-to-docs` |
| Documentazione del branch prodotta | `docs/documentation/{specname}-docs.md` e, se confermato, changelog | `validate-traceability` |
| Tracciabilità validata | report `COERENTE` | nessun passaggio obbligatorio; eventuale nuovo lavoro parte da un nuovo intent |

`review-implementation` è raccomandata ma può essere saltata solo se l'utente lo decide. `close-issue` prepara testo per commit e chiusura, ma non esegue tali operazioni. `branch-to-docs` chiede separatamente se il branch è chiuso prima di produrre il changelog. `validate-traceability` è un controllo finale e non corregge le lacune, è raccomandata ma può essere saltata solo se l'utente lo decide.

## Procedura

1. Riassumere brevemente ciò che l'utente dichiara di avere completato, distinguendo fatti dichiarati da fatti verificati.
2. Confrontare la descrizione con il flusso di riferimento e individuare l'ultimo punto certamente raggiunto.
3. Se serve, controllare in sola lettura l'esistenza degli artefatti locali attesi con `rg --files`, `git status`, `git log` o comandi equivalenti. Non leggere o elencare l'intero repository senza necessità.
4. Non considerare sufficiente il solo nome di un file: quando il passaggio dipende dal contenuto, verificare che il documento abbia almeno il riferimento e le sezioni essenziali previsti dalla skill che lo produce.
5. Distinguere tra prossimo passaggio principale, prerequisiti e attività opzionali. Non proporre una lista di skill alternative se una sola è chiaramente successiva.
6. Se manca un prerequisito bloccante, indicarlo prima della skill successiva e chiedere all'utente di completarlo o fornire il riferimento mancante.
7. Se due punti sono entrambi plausibili, chiedere una domanda mirata, per esempio se l'implementazione è terminata o se il recap è già stato scritto.
8. Rispondere con il formato fisso sotto. Terminare con un prompt breve che l'utente può usare per avviare la prossima skill, senza avviarla automaticamente.

## Formato fisso della risposta

Usare questa struttura:

```markdown
📍 Punto raggiunto

...

🔎 Evidenze

- ...

⏭ Prossima skill

`nome-skill`

Obiettivo: ...

📚 Prerequisiti

- ...

📜 Prompt suggerito

> ...

📦 Note o blocchi

...
```

La sezione `Note o blocchi` deve indicare soltanto elementi utili a procedere; lasciarla vuota se non ci sono. Se non è possibile individuare il punto raggiunto, sostituire `Prossima skill` con una domanda mirata e non inventare il passaggio.

## Regole vincolanti

- Tutto il testo deve essere in italiano, mantenendo invariati i nomi delle skill, i percorsi, i comandi e i riferimenti tecnici.
- Proporre una sola prossima skill principale, salvo ambiguità reale.
- Non dichiarare completato un passaggio sulla base del solo messaggio dell'utente quando manca un artefatto dichiarato come necessario; indicarlo come informazione da confermare.
- Non confondere `close-issue` con la chiusura effettiva della issue: la skill prepara contenuti in chat.
- Non confondere `branch-to-docs` con la chiusura automatica del branch: il changelog richiede conferma dell'utente.
- Non saltare `review-implementation` senza dichiararlo come scelta dell'utente o come passaggio opzionale.
- Non chiedere di eseguire una skill se il relativo prerequisito è mancante e non è stato fornito.
- Non creare file, modificare documenti, implementare codice, eseguire test o compiere operazioni remote.
- Se il flusso è concluso, dirlo esplicitamente invece di proporre una skill non necessaria.

## Verifica finale

Prima di rispondere:

1. Controllare che il punto raggiunto sia supportato dalla descrizione o da evidenze locali.
2. Controllare che la prossima skill sia la prima azione coerente ancora non completata.
3. Controllare che ogni prerequisito indicato sia realmente necessario.
4. Verificare che il prompt suggerito contenga gli input già disponibili e chieda soltanto quelli mancanti.
5. Verificare di non avere creato file o modificato lo stato del repository.

## Errori comuni

- Proporre `implement-issue` quando esiste soltanto un intent e manca la spec o la proposta di issue.
- Proporre `close-issue` senza recap di implementazione o prima di una review richiesta dall'utente.
- Proporre `branch-to-docs` per una singola modifica ancora in corso.
- Proporre il changelog senza la conferma che il branch è chiuso.
- Proporre `validate-traceability` prima che esistano gli artefatti che si vogliono validare.
- Elencare tutto il flusso invece di indicare il prossimo passaggio.
- Eseguire la skill successiva invece di limitarsi a guidare l'utente.
