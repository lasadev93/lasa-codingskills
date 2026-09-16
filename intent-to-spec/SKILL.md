---
name: intent-to-spec
description: Usare quando esiste un file intent.md e serve trasformarlo in una specifica di requisiti e progettazione integrabile nella codebase e pronta per la suddivisione in issue GitHub.
---

# Da intent a specifica

## Scopo

Trasformare un intent esistente in una specifica completa di requisiti e progettazione per l'integrazione nella codebase corrente. Il risultato deve essere leggibile, verificabile e pronto per essere suddiviso in issue GitHub.

## Quando usarla

- È stato fornito o indicato un file `intent-{specname}.md`.
- Serve definire requisiti, impatti, design, dipendenze e piano di implementazione.
- La specifica deve rispettare codebase esistente, brand, sicurezza, privacy e UX.

Non implementare il codice e non creare direttamente issue GitHub: produrre la specifica.

## Procedura

1. Leggere per intero il file intent indicato. Se il file non è raggiungibile o non è un intent, fermarsi e segnalarlo.
2. Derivare `specname` dal nome `intent-{specname}.md` e usare `docs/specs/spec-{specname}.md` come destinazione.
3. Esaminare la codebase interessata: struttura, moduli esistenti, contratti, test, configurazione e documentazione pertinente. Riutilizzare convenzioni e componenti già presenti.
4. Consultare `CONTEXT-MAP.md`, il contesto del dominio interessato, gli ADR rilevanti e la documentazione in `docs/documentation` come baseline.
5. Individuare le decisioni architetturali candidate a un ADR, per esempio scelte con impatto durevole su architettura, contratti, dati, sicurezza, integrazioni o pattern trasversali. Per ogni candidato descrivere contesto, decisione proposta, alternative e conseguenze.
6. Se esiste un candidato ADR, presentarlo all'utente e chiedere conferma esplicita prima di inserirlo. Dopo la conferma, creare `docs/adr` se non esiste e scrivere il nuovo ADR rispettando la convenzione degli ADR già presenti. Se gli ADR esistenti usano numerazione progressiva, individuare il numero più alto, assegnare il successivo e verificare che il percorso non esista già. Se non esiste una convenzione, usare la struttura indicata sotto. Se l'utente non conferma, non creare il file e mantenere la decisione come proposta o domanda aperta nella spec.
7. Separare requisiti derivati dall'intent, osservazioni della codebase e proposte di progettazione. Etichettare le proposte come `Proposta:` e le assunzioni non confermate come `Assunzione:`.
8. Rendere espliciti rischi, dipendenze, criteri di accettazione, verifiche e conflitti tra politiche. Se due vincoli non possono essere soddisfatti insieme, non risolvere il conflitto in silenzio: documentare l'impatto e la decisione necessaria.
9. Creare il file di destinazione. Se esiste già, leggerlo e non sovrascriverlo senza conferma esplicita.

## Contratto del file

Il file deve essere `docs/specs/spec-{specname}.md`, senza date o prefissi aggiuntivi. Usare questa struttura minima, nell'ordine indicato; aggiungere sottosezioni solo quando servono alla funzionalità:

```markdown
# Spec: {feature name}
Fonte: `docs/intents/intent-{specname}.md`

## Obiettivo

## Requisiti

### Funzionali

### Non funzionali

## Integrazione nella codebase

## Progettazione proposta

### Flussi e stati

### Contratti, dati e interfacce

### Impatto e dipendenze

## UX, brand e accessibilità

## Sicurezza e privacy

## Decisioni architetturali e ADR

## Piano di implementazione

## Criticità e conflitti

## Domande aperte
```

## Requisiti di qualità

- Mantenere la tracciabilità dall'intent a ogni requisito e dal requisito alla proposta di design e alla verifica.
- Descrivere l'integrazione con percorsi e file esistenti, senza progettare un sistema parallelo o aggiungere dipendenze non motivate.
- Descrivere UX, accessibilità e coerenza visuale solo sulla base delle linee guida e dei componenti disponibili.
- Per sicurezza e privacy indicare dati trattati, confini di fiducia, autorizzazioni, logging, retention e rischi pertinenti.
- Nella sezione `Criticità e conflitti` usare una tabella con almeno: identificativo, vincoli in conflitto, impatto, opzioni e decisione o blocco richiesto.
- Per ogni possibile ADR esplicitare perché la decisione ha impatto architetturale e riportare nella spec il percorso dell'ADR dopo la conferma e la creazione.
- Non dichiarare approvata una decisione non presente nelle fonti. Riportare le questioni irrisolte in `Domande aperte`.

## Struttura dell'ADR

Quando il repository non fornisce una struttura esistente, usare:

```markdown
# ADR: {titolo}

Stato: Proposto o Accettato.
Fonte: `docs/specs/spec-{specname}.md`

## Contesto

## Decisione

## Alternative considerate

## Conseguenze

## Collegamenti
```

Non usare `Stato: Accettato.` se l'utente ha confermato soltanto la registrazione dell'ADR ma non la decisione tecnica. In quel caso usare `Stato: Proposto.` e lasciare esplicita la decisione necessaria.

## Errori comuni

- Riassumere l'intent senza progettare l'integrazione concreta nella codebase.
- Inventare endpoint, componenti, policy o criteri non supportati dalle fonti.
- Confondere una proposta tecnica con un requisito già deciso.
- Nascondere un conflitto tra brand, UX, sicurezza o vincoli tecnici invece di renderlo visibile.
- Scrivere un piano generico non divisibile in issue verificabili.
- Creare un ADR per ogni dettaglio locale o senza conferma esplicita dell'utente.
