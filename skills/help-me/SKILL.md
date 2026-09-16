---
name: help-me
description: Guida al percorso del pack dalle note grezze alla documentazione del branch.
disable-model-invocation: true
---

# Usare il pack di skill

`help-me` è la router skill del pack. Usala per capire quale skill invocare, quale file indicare e quale risultato aspettarti. È una guida: non invoca automaticamente la skill successiva e non esegue operazioni remote.

Se non viene indicato un punto del percorso, presenta il percorso completo sotto. Se l'utente indica già un artefatto, una issue o un branch, concentra la risposta sul primo passaggio ancora necessario e fornisci il prompt pronto da copiare. La risposta è completa quando contiene la skill da usare, il suo input minimo, l'output atteso e gli eventuali passaggi manuali o prerequisiti.

## Percorso principale

```text
note grezze
    ↓
capture-intent → intent
    ↓
intent-to-spec → spec
    ↓
spec-to-issue → proposte di issue Markdown
    ↓
creazione manuale su GitHub → ID della issue
    ↓
implement-issue → codice, test e recap
    ↓
review-implementation (opzionale)
    ↓
close-issue → Conventional Commit e commento di chiusura
    ↓
branch-to-docs → documentazione del branch e, dopo conferma, changelog
    ↓
validate-traceability (opzionale, raccomandata)
```

I nomi tra parentesi graffe sono valori da ricavare dai file o dal contesto: `{specname}`, `{issue}`, `{branchname}`.

## 1. Catturare l'intento

Invoca `$capture-intent` con le note di ciò che vuoi implementare. Le note possono essere non strutturate, ma devono distinguere per quanto possibile:

- obiettivo o problema da risolvere;
- vincoli già noti;
- cosa si vuole ottenere;
- cosa resta fuori ambito.

Prompt di partenza:

```text
$capture-intent

Nome funzionalità: <nome>
Obiettivo: <risultato desiderato o problema>
Vincoli: <vincoli tecnici, di prodotto, sicurezza o ambito>
Voglio: <comportamenti o risultati richiesti>
Non voglio: <comportamenti o risultati esclusi>
Note aggiuntive:
<appunti liberi>
```

La skill produce `docs/intents/intent-{specname}.md`, se la cartella prevista esiste o ne è stata autorizzata la creazione. Leggi l'intero file appena creato. Se `## Domande aperte` contiene domande, rispondi direttamente nello stesso file prima di passare alla specifica; mantieni distinguibili requisiti confermati e decisioni ancora aperte.

Dettagli: [`capture-intent`](../capture-intent/SKILL.md).

## 2. Trasformare l'intento in specifica

Dopo avere letto e completato l'intent, indica il suo percorso a `$intent-to-spec`:

```text
$intent-to-spec

File intent: docs/intents/intent-{specname}.md
```

La skill analizza anche la codebase e genera `docs/specs/spec-{specname}.md`. Leggi la spec per intero prima di procedere. Se emergono domande aperte, conflitti o candidati ADR, risolvili o confermali secondo quanto richiesto dalla skill; la spec deve essere una base comprensibile e divisibile, non un'approvazione implicita di decisioni non confermate.

Dettagli: [`intent-to-spec`](../intent-to-spec/SKILL.md).

## 3. Preparare le issue

Quando la spec è stata letta, invoca `$spec-to-issue` indicando il file:

```text
$spec-to-issue

File spec: docs/specs/spec-{specname}.md
```

La skill sceglie una sola unità di lavoro autonoma e produce una proposta Markdown, normalmente in `docs/issues/issue-{specname}.md`. Per più issue, mantieni una proposta distinta per ogni unità implementabile e verificabile, con una granularità approvata prima della pubblicazione.

Il comportamento predefinito del pack è creare file di proposta, non issue remote: leggi il file, verifica ambito e criteri di accettazione e crea manualmente la issue su GitHub copiandone il contenuto. Conserva l'ID assegnato, per esempio `#123`. Se vuoi che l'agente esegua anche la creazione su GitHub, dichiaralo esplicitamente nel prompt: è un'operazione remota separata e richiede strumenti, autenticazione e autorizzazione adeguati.

Dettagli: [`spec-to-issue`](../spec-to-issue/SKILL.md).

## 4. Implementare una issue

Con la issue già presente su GitHub, basta indicarne l'ID a `$implement-issue`:

```text
$implement-issue

Issue: #123
```

La skill legge la issue e i riferimenti collegati, implementa il lavoro nella codebase con TDD, verifica i criteri di accettazione e produce il recap in `dev/implementation/{specname}/{issue}-implementation-recap.md`.

Non considerare conclusa l'implementazione finché il recap non documenta file modificati, test eseguiti, criteri verificati e attività residue. La skill non crea commit, non modifica o chiude la issue e non esegue push.

Dettagli: [`implement-issue`](../implement-issue/SKILL.md).

## 5. Fare la review, se richiesta

Al termine di `$implement-issue` viene chiesto se vuoi eseguire `$review-implementation`. La review è opzionale, ma quando la esegui devi dichiarare il perimetro Git da confrontare.

Confronto del branch corrente con `main`:

```text
$review-implementation

Issue: #123
Perimetro: feature/<nome-branch>
```

In questo caso la review usa il confronto `main...feature/<nome-branch>`.

Intervallo tra commit o altro perimetro esplicito:

```text
$review-implementation

Issue: #123
Perimetro: <commit-start>..<commit-end>
```

Se vuoi saltarla, dichiaralo e passa a `$close-issue`. Se non indichi il perimetro, la skill può usare il branch corrente rispetto a `main` quando è identificabile; indicarlo esplicitamente rende la review riproducibile.

Dettagli: [`review-implementation`](../review-implementation/SKILL.md).

## 6. Preparare commit e chiusura della issue

Dopo l'implementazione e l'eventuale review, invoca `$close-issue` con uno o più recap:

```text
$close-issue

Recap: dev/implementation/{specname}/{issue}-implementation-recap.md
```

La skill genera un solo messaggio Conventional Commit aggregato. Il messaggio viene prodotto prima dell'hash: crea manualmente il commit usando quel testo, poi restituisci l'hash, per esempio:

```text
Hash commit: 12abcde
```

Con l'hash ricevuto, `$close-issue` genera un commento separato per ogni issue. Copia il commento su GitHub e chiudi manualmente la issue. Se il recap contiene test falliti, criteri non soddisfatti o blocchi, la skill prepara una nota di blocco invece di un falso messaggio di chiusura.

Dettagli: [`close-issue`](../close-issue/SKILL.md).

## 7. Documentare il branch

Quando tutte le issue del branch sono state implementate e i riferimenti ai commit sono disponibili, invoca `$branch-to-docs` con un branch oppure con un intervallo di commit.

Per un branch:

```text
$branch-to-docs

Branch: feature/<nome-branch>
```

Per un intervallo:

```text
$branch-to-docs

Commit start: <commit-start>
Commit end: <commit-end>
```

La skill confronta il lavoro con `main` e scrive `docs/documentation/{specname}-docs.md`. Dopo la documentazione chiede:

```text
Il branch può ritenersi chiuso? Se sì, preparo anche il changelog confrontato con `main`.
```

Rispondi affermativamente solo quando il branch è davvero chiuso: soltanto allora viene scritto `docs/changelog/{branchname}-docs.md`. La skill non crea commit, non fa push e non chiude il branch.

Dettagli: [`branch-to-docs`](../branch-to-docs/SKILL.md).

## Skill di supporto

- `$whats-next`: indica un solo prossimo passaggio quando descrivi ciò che hai già completato e non sai da dove ripartire. Non invoca la skill suggerita.
- `$validate-traceability`: controlla alla fine la catena intent → spec → issue → implementazione → documentazione e segnala lacune senza correggerle.
- `$keep-a-changelog-from-diff`: supporta direttamente la scrittura di un changelog da un diff; nel percorso principale viene richiamata concettualmente da `branch-to-docs`.

## Regole del percorso

- Leggi ogni artefatto per intero prima di passare alla skill che lo usa.
- Rispondi alle domande aperte nel documento che le contiene, così la decisione resta tracciata.
- Mantieni l'ID GitHub della issue e i percorsi dei recap: sono gli agganci tra progettazione, implementazione e chiusura.
- Specifica sempre il perimetro della review e quello della documentazione quando il branch o l'intervallo non è univoco.
- Tratta la pubblicazione, il commit, il commento e la chiusura su GitHub come passaggi manuali, salvo richiesta esplicita e autorizzazione a eseguire operazioni remote.
- Usa `validate-traceability` quando vuoi controllare che nessun requisito o decisione si sia perso lungo il percorso.
