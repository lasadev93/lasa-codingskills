---
name: capture-intent
description: Usare quando l'intento di una funzionalità è disperso tra punti elenco o appunti grezzi e serve un documento Markdown stabile per prodotto e implementazione.
---

# Catturare l'intento

## Scopo

Raccogliere le note su una funzionalità in un documento Markdown leggibile e prevedibile da elaborare. Conservare ciò che è esplicito e non inventare requisiti.

## Quando usarla

- L'input contiene punti elenco, appunti grezzi o requisiti misti.
- Serve un documento di intent prima della progettazione o implementazione.

Non usarla per specifiche tecniche, user story, suddivisioni in task o ADR.

## Procedura

1. Individuare il nome della funzionalità o derivarlo se possibile; se non è affidabile, chiederlo prima di creare il file.
2. Eliminare i duplicati e raggruppare le note nelle sezioni obbligatorie, con un fatto o outcome per bullet.
4. Non trasformare ipotesi in fatti e tenere distinti problema, outcome, soggetti interessati, vincoli e domande.
5. Se qualcosa non è chiaro, non assumere. Chiedi chiarimenti all'utente.
5. Usare `intent-{featurename}.md` con slug minuscolo in kebab-case: rimuovere accenti e punteggiatura, comprimere i trattini, senza date o prefissi.
6. Scrivere in `docs/intents` solo se esiste. Se manca, non crearla implicitamente ma chiedi autorizzazione a crearla o destinazione alternativa.
7. Riportare il percorso creato senza testo fuori dalla struttura obbligatoria.

## Contratto di output

Usare esattamente questa struttura e questo ordine:

```markdown
# Intent: {feature name}

## Problema

## Outcome proposti

## Utenti e sistemi interessati

## Vincoli

## Domande aperte
```

Regole per le sezioni:

- `Problema`: situazione attuale, bisogno, difficoltà o impatto; non inserire la soluzione.
- `Outcome proposti`: risultati osservabili, con soggetto e verbo chiari quando possibile.
- `Utenti e sistemi interessati`: un bullet per utente, ruolo o sistema, indicando il coinvolgimento.
- `Vincoli`: vincoli espliciti di ambito, tecnici, di sicurezza, normativi, operativi o temporali.
- `Domande aperte`: domande o decisioni irrisolte sulla funzionalità. Lasciare vuota la sezione quando non presenti; non aggiungere domande su autore o stato perché il metadato è `Da definire`.

Preferire bullet brevi con prefissi come `Utente:`, `Sistema:` o `Vincolo:`. Mantenere la lingua originale, salvo richiesta di traduzione.

## Esempio

Per note su un utente che vuole verificare lo stato di una pratica, creare `docs/intents/intent-claims-status-self-service.md`:

```markdown
# Intent: claims status self-service

## Problema

- Gli utenti non possono verificare autonomamente lo stato della propria richiesta.

## Outcome proposti

- Utente: può visualizzare lo stato corrente della richiesta senza contattare l'assistenza.

## Utenti e sistemi interessati

- Utente: richiedente della pratica.
- Sistema: servizio che espone lo stato della pratica.

## Vincoli

- Lo stato mostrato deve provenire dal sistema di gestione delle pratiche.

## Domande aperte

```

## Errori comuni

- Aggiungere criteri di accettazione, dettagli API o task: mantenere l'intent al livello degli outcome.
- Indovinare autore, stato, attori o vincoli: usare solo informazioni supportate dalle note.
- Rinominare intestazioni, aggiungere sezioni o spostare i metadati: la struttura fissa è il contratto automatico.
