---
name: implement-issue
description: Usare quando è disponibile il riferimento a una issue GitHub generata da `spec-to-issue` e occorre implementarla nella codebase esistente con sviluppo TDD.
---

## Accesso ai comandi remoti

Il sandbox potrebbe dare problemi quando si fa `gh issue view` o altri comandi Git che contattano un remote. Quando accade, eseguire subito con accesso esterno (`sandbox_permissions: require_escalated`) ogni comando `gh` e ogni comando `git` che contatta un remote (`fetch`, `pull`, `push`, `clone`, `ls-remote`, ecc.).

# Implementare una issue

## Scopo

Implementare una issue GitHub nel perimetro della proposta generata da `spec-to-issue`, integrandola nella codebase esistente e verificandola con TDD.

## Quando usarla

- È stato fornito un numero, URL o altro riferimento a una issue GitHub.
- La issue deriva da `docs/issues/issue-{specname}.md`.
- Sono richiesti modifiche al codice, test e verifica dei criteri di accettazione.

Non creare, modificare, chiudere o commentare issue GitHub. Non fare commit o push senza richiesta esplicita.

## Procedura

1. Recuperare la issue con `gh issue view <riferimento> --comments`, applicando subito l'accesso esterno indicato sopra.
2. Esaminare `git status`, la struttura della codebase e i percorsi coinvolti. Preservare modifiche non correlate e fermarsi se la issue confligge con lavoro locale non proprio.
3. Consultare gli ADR e le regole di dominio pertinenti.
4. Individuare eventuali nuove decisioni architetturali candidate a un ADR. Se una scelta con impatto durevole riguarda architettura, contratti, dati, sicurezza, integrazioni o pattern trasversali, descrivere contesto, decisione proposta, alternative e conseguenze, chiedere conferma all'utente e attendere prima di adottarla.
5. Tradurre i criteri di accettazione in comportamenti verificabili e scegliere il primo comportamento minimo da implementare.
6. Applicare il ciclo TDD obbligatorio descritto sotto, un comportamento alla volta.
7. Verificare tutti i criteri di accettazione, eseguire i test mirati e poi la suite pertinente più ampia.
8. Fermarsi prima di ampliare lo scope. Se la issue è ambigua, contraddittoria o non implementabile con le fonti disponibili, descrivere il blocco e chiedere chiarimenti.
9. Consegnare riepilogo, file modificati, verifiche eseguite e risultati osservati. Chiedere infine: `Vuoi fare una code review contro la issue?`. Il riepilogo deve essere consegnato in un file `docs/dev/implementation/{specname}/{issue}-implementation-recap.md`. Se la cartella non è presente chiedere autorizzazione a crearla o chiedere percorso alternativo.

Se l'utente conferma la decisione e la registrazione, creare `docs/adr` se non esiste e scrivere il nuovo ADR rispettando la convenzione degli ADR esistenti. Se gli ADR esistenti usano numerazione progressiva, individuare il numero più alto, assegnare il successivo e verificare che il percorso non esista già. Se non esiste una convenzione, usare la struttura nella sezione `Gestione degli ADR`. Se l'utente non conferma e la decisione è necessaria per proseguire, fermarsi; se non è necessaria, mantenerla come proposta o attività residua senza presentarla come approvata.

## TDD obbligatorio

**Nessun codice di produzione prima di un test fallimentare.** Per ogni comportamento:

1. Scrivere un test mirato prima di modificare il codice di produzione.
2. Eseguire il test e verificare che fallisca per l'assenza del comportamento richiesto, non per un errore di sintassi o configurazione.
3. Scrivere il codice minimo per far passare quel test.
4. Se il test fallisce, modificare il codice e non il test. Non indebolire asserzioni, rimuovere casi o alterare il test per ottenere un risultato verde. Se il test è tecnicamente invalido, fermarsi e segnalarlo.
5. Quando il test passa, eseguire la suite pertinente e correggere il codice fino a ottenere risultati verdi.
6. Rifattorizzare solo dopo il verde, senza cambiare il comportamento richiesto, e rieseguire le verifiche.

## Gestione degli ADR

Un ADR è appropriato per una decisione con conseguenze durevoli o trasversali, non per ogni dettaglio di implementazione. Sono esempi: scelta o modifica di un contratto API, modello dati o strategia di persistenza, integrazione esterna, confine di sicurezza, pattern condiviso o compromesso architetturale.

Quando emerge un candidato:

1. Esporre all'utente il contesto, la decisione proposta, le alternative considerate e le conseguenze.
2. Chiedere conferma esplicita della decisione e della sua registrazione in `docs/adr`.
3. Dopo la conferma, creare il nuovo ADR con il percorso conforme alla convenzione del repository, senza sovrascrivere un ADR esistente senza consenso; usare `docs/adr/adr-{adrname}.md` solo se non esiste una convenzione.
4. Collegare l'ADR alla spec, all'issue e al recap di implementazione.
5. Se la decisione non viene confermata, non adottarla tacitamente e riportare il blocco o l'attività residua.

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

Usare `Stato: Accettato.` soltanto se l'utente ha confermato anche la decisione tecnica; la sola conferma della registrazione richiede `Stato: Proposto.`.

Un test che passa immediatamente non dimostra il comportamento nuovo: controllare che stia realmente verificando il requisito prima di procedere.

## Struttura del recap di implementazione

Il recap finale deve essere scritto in `dev/implementation/{specname}/{issue}-implementation-recap.md` usando questa struttura:

```markdown
# Recap implementazione: {feature name}

Issue: #{issue}
Spec: `docs/specs/spec-{specname}.md`
Stato: Completata.

## Riepilogo

## Modifiche effettuate

## File modificati

## Test e verifiche

## Criteri di accettazione

## Decisioni architetturali e ADR

## BREAKING CHANGES

## Criticità e attività residue
```

Compilare le sezioni con dati verificabili:

- `Riepilogo`: risultato ottenuto e perimetro effettivamente implementato.
- `Modifiche effettuate`: comportamento aggiunto o corretto, senza narrazione del lavoro passo per passo.
- `File modificati`: percorsi dei file creati, modificati o rimossi e motivo della modifica.
- `Test e verifiche`: test RED iniziale, test GREEN finale, suite più ampia e controlli manuali o di integrazione, con comandi e risultati.
- `Criteri di accettazione`: elenco dei criteri della issue con esito verificato.
- `Decisioni architetturali e ADR`: ADR creati o decisioni candidate, con percorso e collegamenti; lasciare vuota se non ci sono decisioni architetturali rilevanti.
- `BREAKING CHANGES`: compilare solo per incompatibilità reali verso utenti, API, contratti, configurazioni, dati o integrazioni. Se non esistono, lasciare la sezione vuota: non scrivere `Nessuna`, `N/A` o ipotesi.
- `Criticità e attività residue`: blocchi, limitazioni, decisioni rinviate e lavori non inclusi.

Usare `Stato: Completata.` solo quando i criteri di accettazione e le verifiche sono conclusi; in caso contrario indicare lo stato reale.

## Vincoli di implementazione

- La issue e i suoi criteri di accettazione definiscono lo scope; non aggiungere funzionalità speculative o refactoring non necessari.
- Integrare il comportamento nei moduli, contratti e componenti già presenti prima di introdurre nuovi pattern o dipendenze.
- Mantenere i requisiti di sicurezza, privacy, accessibilità, UX e brand riportati nella issue o nelle fonti collegate.
- Non inventare decisioni mancanti. Segnalare incertezze, conflitti tra politiche e dipendenze bloccanti.
- Non dichiarare la issue completata senza output dei test e verifica dei criteri di accettazione.

## Verifica finale

Prima della consegna controllare:

- ogni criterio di accettazione è coperto da codice e verifica;
- ogni comportamento nuovo ha un test che è stato visto fallire prima dell'implementazione;
- i test mirati e la suite pertinente passano senza errori o avvisi non spiegati;
- il recap esiste nel percorso previsto e documenta criteri, verifiche, file modificati, criticità e `BREAKING CHANGES`;
- ogni ADR emerso durante l'implementazione è stato confermato, creato in `docs/adr` e collegato al recap, oppure è documentato come decisione non confermata;
- `BREAKING CHANGES` è compilata soltanto in presenza di incompatibilità reali;
- il diff contiene solo modifiche necessarie alla issue;
- non sono state modificate le issue GitHub né eseguite operazioni remote non richieste.

## Errori comuni

- Scrivere il codice prima del test o saltare la verifica del fallimento iniziale.
- Cambiare il test perché il codice non lo soddisfa.
- Fermarsi al primo test verde senza eseguire la suite pertinente.
- Implementare la spec completa quando la issue copre una sola unità di lavoro.
- Ignorare commenti, ADR o vincoli di sicurezza collegati alla issue.
- Adottare una decisione architetturale nuova senza confermarla con l'utente o senza registrarla in `docs/adr`.
- Dimenticare di chiedere se la code review contro la issue è desiderata.
- Non fornire il file `dev/implementation/{specname}/{issue}-implementation-recap.md`.
