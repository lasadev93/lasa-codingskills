---
name: spec-to-issue
description: Usare quando è disponibile una specifica `spec-{specname}.md` e serve preparare una proposta di issue GitHub pronta per l'implementazione.
---

# Da specifica a proposta di issue

## Scopo

Trasformare una specifica generata da `intent-to-spec` in un documento Markdown utilizzabile per creare una issue GitHub. Il documento deve conservare la tracciabilità, rendere il lavoro azionabile e proporre una granularità verificabile.

## Quando usarla

- È stato fornito o indicato un file `spec-{specname}.md`.
- La specifica deve essere trasformata in una proposta di issue.
- Serve preparare il contenuto prima della pubblicazione nel tracker.

Non creare issue GitHub, non usare `gh` e non implementare il codice.

## Procedura

1. Leggere per intero la specifica indicata. Se non è raggiungibile fermarsi e segnalarlo.
2. Leggere anche l'intent citato dalla specifica per preservare il collegamento tra problema, outcome e lavoro proposto.
3. Esaminare nella codebase solo il contesto necessario a chiarire file, moduli, contratti, test, dipendenze e convenzioni già esistenti.
4. Scegliere una sola unità di lavoro autonoma, implementabile e verificabile. Raggruppare attività inseparabili; lasciare fuori ciò che ha valore o dipendenze indipendenti. Esplicitare la motivazione in `Obiettivo della suddivisione`.
5. Trasferire nella proposta soltanto requisiti e decisioni supportati dalla spec, dall'intent o dalla codebase. Marcare come `Proposta:` ogni scelta aggiunta per l'implementazione e come `Assunzione:` ciò che richiede conferma.
6. Mantenere i vincoli di UX, brand, accessibilità, sicurezza e privacy già definiti nella spec.
7. Rendere i criteri di accettazione osservabili e le verifiche eseguibili. Non inventare comandi, endpoint, numeri di issue, assegnatari o approvazioni.
8. Se non esiste, chiedere autorizzazione a creare `docs/issues` o destinazione alternativa e scrivere `docs/issues/issue-{specname}.md`. Se il file esiste già, leggerlo e non sovrascriverlo senza conferma esplicita.
9. Dopo la scrittura, chiedere esattamente: `La granularità proposta va bene o preferisci accorpare o suddividere la issue?`

## Struttura fissa del documento

Usare esattamente questa struttura e questo ordine:

```markdown
# Proposta di issue: {feature name}

Fonte: `docs/specs/spec-{specname}.md`
Stato: Proposta.

## Obiettivo della suddivisione

## Issue proposta

### Titolo

### Contesto

### Obiettivo

### Ambito

### Fuori ambito

### Requisiti funzionali

### Requisiti non funzionali

### Integrazione nella codebase

### UX, accessibilità e brand

### Sicurezza e privacy

### Dipendenze e prerequisiti

### Criteri di accettazione

### Verifiche richieste

### Criticità e domande aperte
```

## Regole di compilazione

- `Obiettivo della suddivisione` spiega perché questa unità è la granularità proposta e indica eventuali lavori da trattare separatamente.
- `Titolo` è breve, azionabile e descrive il risultato, non il dettaglio tecnico.
- `Ambito` contiene solo il lavoro necessario per l'obiettivo; `Fuori ambito` protegge i confini della issue.
- Ogni requisito e criterio di accettazione descrive un solo risultato. Usare checklist per i criteri e formulazioni verificabili.
- `Integrazione nella codebase` cita percorsi, componenti, API, modelli o test esistenti solo quando sono supportati dall'analisi.
- `Dipendenze e prerequisiti` indica ordine, blocchi e decisioni esterne; non simulare numeri o collegamenti GitHub.
- `Criticità e domande aperte` conserva conflitti e questioni irrisolte della spec. Un conflitto tra politiche non va risolto tacitamente.
- Se una sezione non ha contenuti, mantenerne l'intestazione e lasciarla vuota; non riempirla con testo generico.
- Il documento è una proposta: non presentare come approvati contenuti che la spec lascia aperti.

## Errori comuni

- Copiare la spec senza ridurre il lavoro a un'unità implementabile.
- Dividere artificialmente attività che devono essere rilasciate e verificate insieme.
- Perdere vincoli di sicurezza, privacy, UX o accessibilità durante la sintesi.
- Inserire dettagli tecnici inventati per rendere l'issue apparentemente completa.
- Creare o modificare issue GitHub invece del solo documento Markdown.
- Dimenticare la domanda finale sulla granularità proposta.
