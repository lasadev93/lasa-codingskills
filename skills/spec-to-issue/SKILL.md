---
name: spec-to-issue
description: Usare quando è disponibile una specifica `spec-{specname}.md` e serve preparare una sequenza completa di proposte di issue GitHub pronte per l'implementazione.
---

# Da specifica a sequenza di proposte di issue

## Scopo

Trasformare una specifica generata da `intent-to-spec` in una sequenza ordinata di documenti Markdown utilizzabili per creare issue GitHub. La sequenza deve coprire l'intera spec e ogni documento deve descrivere un'unità atomica, implementabile e verificabile da un coding agent. L'ultima issue deve portare al completamento della spec; non fermarsi alla prima fondazione tecnica.

## Quando usarla

- È stato fornito o indicato un file `spec-{specname}.md`.
- La specifica deve essere trasformata in più proposte di issue, oppure la sua copertura completa richiede più unità di lavoro.
- Serve preparare il contenuto prima della pubblicazione nel tracker.

Non creare issue GitHub, non usare `gh` e non implementare il codice.

## Procedura

1. Leggere per intero la specifica indicata. Se non è raggiungibile fermarsi e segnalarlo.
2. Leggere anche l'intent citato dalla specifica per preservare il collegamento tra problema, outcome e lavoro proposto.
3. Esaminare nella codebase solo il contesto necessario a chiarire file, moduli, contratti, test, dipendenze e convenzioni già esistenti.
4. Scomporre l'intera spec in una sequenza di unità autonome, implementabili e verificabili. Identificare prima outcome, requisiti e dipendenze; poi raggruppare nella stessa issue solo le attività inseparabili per implementazione o verifica. Creare un'issue distinta per ogni unità che abbia valore o dipendenze indipendenti.
5. Ordinare le issue secondo le dipendenze reali e verificare che la sequenza non abbia lacune né sovrapposizioni: ogni requisito della spec deve essere coperto da almeno una issue e l'ultima issue deve includere il lavoro necessario a completare la spec. Una sola issue è ammessa solo se la spec è realmente indivisibile o l'utente chiede esplicitamente una singola issue.
6. Per ogni issue, definire confini, predecessori e risultato verificabile. Indicare in `Obiettivo della suddivisione` perché quella unità ha quella granularità e quali issue precedenti o successive la collegano.
7. Trasferire nelle proposte soltanto requisiti e decisioni supportati dalla spec, dall'intent o dalla codebase. Marcare come `Proposta:` ogni scelta aggiunta per l'implementazione e come `Assunzione:` ciò che richiede conferma.
8. Mantenere i vincoli di UX, brand, accessibilità, sicurezza e privacy già definiti nella spec.
9. Rendere i criteri di accettazione osservabili e le verifiche eseguibili. Non inventare comandi, endpoint, numeri di issue, assegnatari o approvazioni.
10. Se non esiste, chiedere autorizzazione a creare `docs/issues` o destinazione alternativa. Scrivere un file Markdown separato per ogni issue, usando il nome `docs/issues/issue-{specname}-{NN}-{slug}.md`, dove `NN` è l'ordine sequenziale (01, 02, ...) e non un numero GitHub. Se un file di destinazione esiste già, leggerlo e non sovrascriverlo senza conferma esplicita.
11. Dopo la scrittura, chiedere esattamente: `La sequenza e la granularità proposte vanno bene o preferisci accorpare o suddividere qualche issue?`

## Struttura fissa del documento

Ripetere questa struttura, nello stesso ordine, in ogni file della sequenza. Sostituire `{N}` e `{totale}` con l'ordine e il numero totale delle issue; il numero deve comparire anche nel nome del file per rendere evidente la sequenza.

```markdown
# Proposta di issue {N}/{totale}: {feature name}

Fonte: `docs/specs/spec-{specname}.md`
Stato: Proposta.
Sequenza: {N} di {totale}.

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

- `Obiettivo della suddivisione` spiega perché questa unità è la granularità proposta, indica i predecessori e successori rilevanti e chiarisce come contribuisce al completamento dell'intera spec.
- `Titolo` è breve, azionabile e descrive il risultato, non il dettaglio tecnico.
- `Ambito` contiene solo il lavoro necessario per l'obiettivo; `Fuori ambito` protegge i confini della issue.
- Ogni requisito e criterio di accettazione descrive un solo risultato. Usare checklist per i criteri e formulazioni verificabili.
- `Integrazione nella codebase` cita percorsi, componenti, API, modelli o test esistenti solo quando sono supportati dall'analisi.
- `Dipendenze e prerequisiti` indica l'ordine nella sequenza, i blocchi e le decisioni esterne; non simulare numeri o collegamenti GitHub.
- `Criticità e domande aperte` conserva conflitti e questioni irrisolte della spec. Un conflitto tra politiche non va risolto tacitamente.
- Se una sezione non ha contenuti, mantenerne l'intestazione e lasciarla vuota; non riempirla con testo generico.
- Il documento è una proposta: non presentare come approvati contenuti che la spec lascia aperti.
- Prima di concludere, controllare la copertura: nessun requisito della spec deve restare senza issue assegnata e nessuna issue deve introdurre lavoro non collegato alla spec.

## Errori comuni

- Copiare la spec senza ridurre il lavoro a unità implementabili.
- Creare solo la prima issue fondativa e lasciare come lavoro successivo non formalizzato il resto della spec.
- Produrre issue indipendenti senza ordine, dipendenze o verifica della copertura completa.
- Dividere artificialmente attività che devono essere rilasciate e verificate insieme.
- Perdere vincoli di sicurezza, privacy, UX o accessibilità durante la sintesi.
- Inserire dettagli tecnici inventati per rendere l'issue apparentemente completa.
- Creare o modificare issue GitHub invece del solo documento Markdown.
- Dimenticare la domanda finale sulla sequenza e sulla granularità proposta.
