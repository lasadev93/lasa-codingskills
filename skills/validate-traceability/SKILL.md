---
name: validate-traceability
description: Usare quando occorre verificare la coerenza e la copertura della catena intent, spec, issue, implementazione, documentazione, ADR e changelog.
---

# Verificare la tracciabilità

## Scopo

Controllare che le decisioni e i requisiti di una funzionalità siano rintracciabili dall'intent fino all'implementazione e alla documentazione finale. La skill individua artefatti mancanti, collegamenti deboli, requisiti senza copertura, contraddizioni e dichiarazioni prive di evidenza.

La skill produce esclusivamente un report di tracciabilità in chat. Non crea, aggiorna o sposta documenti, non modifica codice e non esegue operazioni Git o GitHub che cambiano stato.

## Quando usarla

- Prima di chiudere una issue o un branch.
- Dopo `branch-to-docs`, per controllare documentazione e changelog.
- Quando più issue o recap derivano dalla stessa spec.
- Quando serve verificare che un requisito, un ADR o un breaking change non sia stato perso durante il flusso.

## Input

Richiedere almeno uno `specname` oppure il percorso di un intent o di una spec:

```text
Specname: claims-status-self-service
```

Accettare facoltativamente un branch o un intervallo di commit per corroborare l'implementazione con Git:

```text
Branch: feature/claims-status-self-service
```

oppure:

```text
Commit start: abc1234
Commit end: def5678
```

Se l'input non consente di identificare una funzionalità o più spec sono candidate, chiedere il perimetro senza scegliere arbitrariamente.

## Catena da controllare

Per `specname`, esaminare gli artefatti secondo questa catena:

```text
docs/intents/intent-{specname}.md
        ↓
docs/specs/spec-{specname}.md
        ↓
docs/issues/issue-{specname}.md
        ↓
dev/implementation/{specname}/{issue}-implementation-recap.md
        ↓
docs/documentation/{specname}-docs.md
        ↓
docs/changelog/{branchname}-docs.md   (solo se il branch è chiuso)
```

Controllare anche gli ADR in `docs/adr` citati o implicati dagli artefatti. Se la spec è stata suddivisa in più issue o se esistono più recap, includerli tutti e mantenerne l'associazione con la stessa spec.

## Procedura

1. Individuare i percorsi attesi e verificare l'esistenza dei file. Non creare i file mancanti e non considerare valido un riferimento basato soltanto sul nome.
2. Leggere per intero gli artefatti esistenti. Consultare la documentazione secondo la priorità `docs/00-official/` → `docs/01-inbox-updates/` → `docs/02-working-notes/` → `docs/04-reference/` → `docs/05-drafts/`; non usare `docs/03-history/` come baseline.
3. Verificare i collegamenti espliciti: fonte dell'intent nella spec, fonte della spec nell'issue, issue e spec nei recap, perimetro Git nella documentazione e confronto con `main` nel changelog.
4. Estrarre i requisiti funzionali e non funzionali dall'intent e dalla spec. Per ciascuno cercare una copertura nell'issue, nei criteri di accettazione, nel recap e nella documentazione dell'implementazione.
5. Verificare che ogni criterio di accettazione abbia un esito nel recap e che l'esito sia supportato da test o altre verifiche dichiarate. Distinguere `soddisfatto`, `parziale`, `non soddisfatto` e `non verificabile`.
6. Confrontare implementazione, recap e documentazione per rilevare discrepanze su comportamento, file modificati, test, criticità, attività residue e `BREAKING CHANGES`.
7. Verificare gli ADR: ogni decisione architetturale citata deve puntare a un file in `docs/adr`; ogni ADR deve rendere identificabili contesto, decisione, alternative e conseguenze, anche quando il repository usa una forma narrativa senza queste intestazioni letterali. Rispettare la convenzione documentale esistente e segnalare decisioni importanti presenti solo in modo implicito.
8. Se è indicato un branch o un intervallo, usare Git in sola lettura per corroborare file, commit e perimetro. Non includere modifiche locali non committate come implementazione conclusa.
9. Se esiste un changelog, verificare che sia stato creato solo per un branch dichiarato chiuso, che confronti il risultato con `main`, che usi categorie Keep a Changelog corrette e che non introduca voci non supportate dalla documentazione o dal diff.
10. Produrre il report in chat con esito complessivo e azioni necessarie. Non correggere gli artefatti durante la validazione.

## Stati di validazione

Usare soltanto questi stati:

- `OK`: artefatto o collegamento presente e supportato.
- `MANCANTE`: artefatto o informazione attesa assente.
- `INCOERENTE`: artefatti presenti ma in conflitto tra loro.
- `NON VERIFICABILE`: informazione dichiarata senza evidenza sufficiente.
- `NON APPLICABILE`: artefatto opzionale non richiesto dal flusso, per esempio un changelog per un branch non chiuso.

L'esito complessivo è:

- `COERENTE` se non ci sono elementi `MANCANTE`, `INCOERENTE` o `NON VERIFICABILE` rilevanti;
- `ATTENZIONE` se esistono lacune non bloccanti;
- `BLOCCATO` se manca un artefatto fondamentale, c'è un conflitto irrisolto o non è possibile stabilire il perimetro.

## Formato fisso del report

Restituire in chat questa struttura:

```markdown
# Validazione tracciabilità: {specname}

Perimetro Git: `{branch, intervallo oppure non indicato}`
Esito: `COERENTE | ATTENZIONE | BLOCCATO`

## Matrice degli artefatti

| Artefatto | Percorso | Stato | Evidenza o problema |
|---|---|---|---|
| Intent | ... | ... | ... |
| Spec | ... | ... | ... |
| Issue | ... | ... | ... |
| Recap | ... | ... | ... |
| Documentazione | ... | ... | ... |
| Changelog | ... | ... | ... |
| ADR | ... | ... | ... |

## Copertura dei requisiti

| Requisito o criterio | Fonti | Implementazione/verifica | Esito |
|---|---|---|---|
| ... | ... | ... | ... |

## Contraddizioni e lacune

## Azioni richieste
```

Per più issue o recap, aggiungere una riga per ciascun artefatto e indicare chiaramente quale requisito o issue è coinvolto. Non usare `OK` solo perché un file esiste: controllarne contenuto e riferimenti.

## Regole vincolanti

- Tutto il report deve essere in italiano, mantenendo invariati percorsi, nomi tecnici e categorie Keep a Changelog.
- La validazione è osservazionale: non inventare collegamenti, approvazioni, test, esiti o decisioni.
- Non sostituire una fonte mancante con una deduzione dal nome del file o dal messaggio di commit.
- Non cancellare, spostare o correggere artefatti durante la validazione.
- Non considerare un requisito coperto se è presente nella spec ma assente da issue, implementazione o verifica.
- Non considerare un breaking change reale se è soltanto ipotizzato; segnalare invece l'incoerenza se un recap lo dichiara ma la documentazione finale lo omette.
- Non considerare un ADR necessario per ogni dettaglio tecnico locale; segnalarlo solo quando esiste una decisione architetturale con impatto durevole, trasversale o su contratti, dati, sicurezza o integrazioni.
- Se le fonti del repository sono in conflitto, segnalarlo come `INCOERENTE` e chiedere una decisione.

## Verifica finale

Prima di consegnare:

1. Controllare che ogni artefatto atteso abbia uno stato.
2. Controllare che ogni requisito e criterio abbia una copertura o una lacuna esplicita.
3. Verificare che gli ADR citati abbiano un percorso e un contenuto coerenti.
4. Verificare che l'esito complessivo sia coerente con le anomalie trovate.
5. Ricordare che il report è stato prodotto in chat e che nessun artefatto è stato modificato.

## Errori comuni

- Confondere la presenza di un riferimento con la copertura del requisito.
- Considerare completato un criterio solo perché il recap lo dichiara.
- Ignorare differenze tra spec, issue, codice, test e documentazione finale.
- Trattare un changelog non applicabile come un documento mancante.
- Dimenticare gli ADR o i breaking changes durante il controllo di tracciabilità.
- Correggere i documenti invece di riportare le lacune.
