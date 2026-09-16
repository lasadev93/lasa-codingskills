---
name: review-implementation
description: Usare quando un'implementazione deve essere verificata contro una issue GitHub, la spec collegata, i criteri di accettazione e gli standard della codebase.
---

# Verificare un'implementazione

## Accesso ai comandi remoti

Il sandbox potrebbe dare problemi quando si fa `gh issue view` o altri comandi Git che contattano un remote. Quando accade, eseguire subito con accesso esterno (`sandbox_permissions: require_escalated`) ogni comando `gh` e ogni comando `git` che contatta un remote (`fetch`, `pull`, `push`, `clone`, `ls-remote`, ecc.).

## Scopo

Verificare se l'implementazione associata a una issue soddisfa effettivamente la issue, la spec e i criteri di accettazione, tenendo conto della codebase esistente, delle politiche di sicurezza, dei requisiti UX e dei test.

La skill produce esclusivamente un report di code review in chat. Non modifica codice, test o documenti, non commenta la issue e non crea commit.

## Quando usarla

- `implement-issue` ha terminato l'implementazione e l'utente ha richiesto una code review.
- Esiste un riferimento a una issue GitHub e occorre verificare il lavoro prima di chiudere la issue o il branch.
- Serve una valutazione indipendente dei criteri di accettazione e delle criticità residue.

## Input

Richiedere almeno il riferimento alla issue:

```text
Issue: #12
```

Accettare anche URL o altri riferimenti comprensibili da `gh issue view`. Se l'utente indica un perimetro, usare quello:

```text
Perimetro: feature/nome-branch
```

oppure:

```text
Perimetro: commit-start..commit-end
```

Se il perimetro non è indicato, esaminare il branch corrente rispetto a `main`, includendo le modifiche staged e unstaged presenti nella working tree. Se il branch corrente è `main`, se `main` non è disponibile o se il lavoro da valutare non è identificabile, fermarsi e chiedere il perimetro.

## Procedura

1. Recuperare la issue con `gh issue view <riferimento> --comments`, usando subito l'accesso esterno indicato sopra. Leggere titolo, descrizione, criteri, commenti e riferimenti a spec o recap.
2. Leggere per intero la spec collegata, l'intent, la proposta di issue e i recap di implementazione disponibili. Se manca una fonte dichiarata dalla issue, segnalarlo senza ricostruirla arbitrariamente.
3. Verificare il perimetro Git. Per un branch usare il confronto `main...branch`; per un intervallo usare i commit indicati. Separare modifiche committate da modifiche locali e dichiarare entrambe nel report.
4. Esaminare diff, log, file modificati, contratti, configurazione, migrazioni, test e documentazione pertinente. Consultare la documentazione secondo la priorità `docs/00-official/` → `docs/01-inbox-updates/` → `docs/02-working-notes/` → `docs/04-reference/` → `docs/05-drafts/`; non usare `docs/03-history/` come baseline.
5. Mappare ogni criterio di accettazione a implementazione ed evidenza. Un criterio è soddisfatto soltanto quando il diff e una verifica osservabile lo supportano.
6. Eseguire, quando i comandi sono già identificabili e non modificano il repository, i test mirati e i controlli pertinenti. Non alterare codice o test per correggere un risultato durante la review.
7. Valutare separatamente correttezza funzionale, regressioni, qualità del design, gestione degli errori, sicurezza e privacy, accessibilità, UX, brand, performance e manutenibilità. Considerare solo gli aspetti pertinenti al lavoro.
8. Classificare i rilievi come `BLOCKER`, `MAJOR`, `MINOR` o `NOTE`, indicando evidenza, impatto e correzione richiesta. Non trasformare preferenze stilistiche in blocchi.
9. Formulare un verdetto: `APPROVATA` solo se tutti i criteri sono verificati, non esistono rilievi `BLOCKER` o `MAJOR` e le verifiche pertinenti sono concluse; `RICHIEDE MODIFICHE` se l'implementazione è valutabile ma presenta rilievi da correggere; `BLOCCATA` se mancano riferimenti, baseline o informazioni necessarie alla valutazione.

## Formato fisso del report

Restituire in chat questa struttura:

```markdown
# Code review: issue #{issue}

Perimetro: `{branch oppure intervallo}`
Verdetto: `APPROVATA | RICHIEDE MODIFICHE | BLOCCATA`

## Sintesi

## Criteri di accettazione

| Criterio | Evidenza | Esito |
|---|---|---|
| ... | ... | Soddisfatto / Parziale / Non verificato / Non soddisfatto |

## Rilievi

### BLOCKER

### MAJOR

### MINOR

### NOTE

## Verifiche eseguite

## Sicurezza e privacy

## UX, accessibilità e brand

## Decisione finale
```

Riempire le sezioni con fatti e riferimenti a file, simboli, test o output osservabili. Lasciare vuote le categorie di rilievi senza contenuti; non scrivere `Nessuno` se non serve. Se un test è presente nel diff ma non è stato eseguito, indicarlo come test disponibile, non come verifica superata.

## Regole vincolanti

- Tutto il report deve essere in italiano, mantenendo invariati i nomi tecnici e i livelli `BLOCKER`, `MAJOR`, `MINOR` e `NOTE`.
- Non implementare correzioni durante la review e non modificare i test per farli passare.
- Non dichiarare approvata un'implementazione con criteri non verificati, test falliti o rilievi `BLOCKER` o `MAJOR`.
- Non inventare requisiti, risultati di test, riferimenti GitHub, file o decisioni architetturali.
- Distinguere difetti osservati, rischi plausibili e semplici raccomandazioni.
- Se viene individuata una possibile decisione architetturale nuova, segnalarla come rilievo o raccomandazione e chiedere di gestirla con il flusso ADR di `intent-to-spec` o `implement-issue`; non creare l'ADR durante la code review.
- Non chiudere, commentare o modificare la issue GitHub.
- Non eseguire commit, push, fetch, pull o altri comandi remoti non richiesti.

## Verifica finale

Prima di consegnare il report:

1. Controllare che ogni criterio della issue abbia un esito.
2. Controllare che ogni rilievo abbia evidenza, gravità e impatto.
3. Separare test eseguiti, test presenti nel diff e verifiche non disponibili.
4. Verificare che il verdetto sia coerente con criteri, test e rilievi.
5. Ricordare che il report è stato prodotto in chat e che non sono state effettuate modifiche.

## Errori comuni

- Approvare il lavoro perché i test passano senza verificare i criteri di accettazione.
- Trattare un test aggiunto come prova della sua esecuzione.
- Dimenticare regressioni su contratti, dati, configurazione o integrazioni esterne.
- Segnalare opinioni di stile come difetti bloccanti.
- Correggere il codice mentre si sta svolgendo una review.
- Usare una baseline Git diversa da quella dichiarata.
