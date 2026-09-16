---
name: branch-to-docs
description: Usare quando un branch o un intervallo di commit è stato implementato e occorre documentarne il risultato e, previa conferma, produrre un changelog confrontato con `main`.
---

# Da branch a documentazione e changelog

## Scopo

Documentare in modo verificabile ciò che è stato implementato in un branch o in un intervallo di commit. Dopo la documentazione, chiedere all'utente se il branch può considerarsi chiuso; creare il changelog soltanto dopo una risposta affermativa.

La skill modifica esclusivamente i documenti Markdown previsti:

- `docs/documentation/{specname}-docs.md`
- `docs/changelog/{branchname}-changelog.md`, soltanto se l'utente conferma la chiusura del branch

Non crea commit, non esegue `git add`, `git commit` o `git push` e non modifica il codice.

## Quando usarla

- L'implementazione di un branch è terminata.
- È disponibile il nome di un branch da analizzare.
- Oppure è disponibile una coppia `commit-start` / `commit-end` che delimita l'implementazione.
- Serve una documentazione stabile dell'implementazione e, se il lavoro è chiuso, un changelog.

## Input richiesto

Accettare uno solo dei seguenti formati:

```text
Branch: feature/nome-branch
```

oppure:

```text
Commit start: abc1234
Commit end: def5678
```

I riferimenti possono essere hash, tag o ref locali risolvibili da Git. Se mancano il branch oppure uno dei due commit, chiederli senza inventare valori.

Per una coppia di commit, considerare inclusi sia il commit iniziale sia quello finale e analizzare l'intervallo `commit-start^..commit-end`. Se il commit iniziale non ha un genitore risolvibile, fermarsi e segnalarlo.

## Procedura

### 1. Validare il perimetro

1. Leggere le istruzioni del repository applicabili al lavoro.
2. Verificare che `main` esista come ref locale. Se non esiste, fermarsi e chiedere come renderlo disponibile; non sostituirlo silenziosamente con un altro branch e non eseguire automaticamente un fetch.
3. Verificare che il branch o entrambi i commit siano risolvibili. Se un riferimento non è valido, fermarsi indicando quale riferimento manca.
4. Controllare lo stato della working tree. Considerare nell'analisi soltanto contenuti committati; segnalare eventuali modifiche locali non incluse nel documento.
5. Per un branch, usare come perimetro funzionale il confronto `main...branch`, cioè le modifiche dal merge base rispetto al branch indicato.
6. Per un intervallo, usare il perimetro `commit-start^..commit-end` per log e diff. Per il successivo changelog, confrontare il commit finale con `main` tramite `main...commit-end`.

I comandi Git di sola lettura ammessi includono `git status`, `git log`, `git show`, `git diff`, `git merge-base` e `git branch`. Non usare il nome di un file o il messaggio di un commit come prova sufficiente dell'implementazione: leggere diff e contenuti pertinenti.

### 2. Ricostruire ciò che è stato implementato

Esaminare, in base al perimetro:

- log e messaggi dei commit;
- statistiche e lista dei file modificati;
- diff completo o diff mirati sui file pertinenti;
- test aggiunti o modificati e risultati disponibili;
- specifica collegata in `docs/specs/`, intent, issue e recap in `dev/implementation/`, se presenti;
- documentazione ufficiale.

Separare sempre:

- comportamento effettivamente implementato;
- decisioni tecniche osservabili nella codebase;
- verifiche realmente eseguite;
- limitazioni, debito tecnico e attività residue.

Non dedurre che un requisito sia soddisfatto solo perché esiste un file, una funzione o un messaggio di commit. Quando una verifica non è disponibile, dichiararlo. Non eseguire test o build salvo richiesta esplicita: questa skill documenta il lavoro osservato nell'intervallo indicato.

### 3. Risolvere i nomi dei documenti

#### `specname`

Usare il suffisso di un file esistente `docs/specs/spec-{specname}.md` quando il collegamento con il branch o l'intervallo è univoco e supportato da commit, issue, recap o contenuto della specifica.

Se il collegamento non è univoco, se sono coinvolte più spec o se non esiste una spec collegabile, chiedere quale `{specname}` usare. Non inventare un nome e non scegliere silenziosamente una spec tra più candidate.

#### `branchname`

Per un branch, usare il nome del branch indicato, rimuovendo eventuali prefissi tecnici non adatti a un nome file e sostituendo `/` con `-`.

Per un intervallo di commit, cercare i branch locali che contengono `commit-end`. Se esiste un solo candidato plausibile, usare il suo nome normalizzato; se non esiste o ce ne sono più di uno, chiedere il `{branchname}` da usare per il changelog. Non usare automaticamente `main` come nome del branch implementato.

Conservare nel contenuto il riferimento originale completo al branch o ai commit, anche quando il nome viene normalizzato per il percorso.

### 4. Scrivere la documentazione dell'implementazione

Creare `docs/documentation` se non esiste e scrivere `docs/documentation/{specname}-docs.md` usando questa struttura fissa:

```markdown
# Documentazione: {feature name}

Fonte: `{branch oppure commit-start^..commit-end}`
Spec: `docs/specs/spec-{specname}.md`
Stato: Documentato.

## Sintesi

## Funzionalità implementate

## Flussi e comportamento

## Modifiche alla codebase

## Contratti, dati e integrazioni

## UX, accessibilità e brand

## Sicurezza e privacy

## Test e verifiche

## Limitazioni, criticità e attività residue
```

Compilare la struttura con fatti tratti dall'analisi. Omettere le sezioni non pertinenti soltanto se la loro assenza non nasconde un'informazione rilevante; in caso di dubbio mantenerle vuote. Nella sezione `Test e verifiche` distinguere i comandi eseguiti e documentati dai test soltanto presenti nel diff. Riportare i breaking changes in `Limitazioni, criticità e attività residue` e descriverne l'impatto concreto.

Se il file di documentazione esiste già, leggerlo e chiedere conferma prima di aggiornarlo. Non sovrascriverlo e non creare una copia con un nome alternativo senza consenso.

### 5. Chiedere se il branch è chiuso

Dopo avere scritto o aggiornato la documentazione, fermarsi e chiedere esattamente:

```text
Il branch può ritenersi chiuso? Se sì, preparo anche il changelog confrontato con `main`.
```

Non creare il changelog prima della conferma. Se l'utente risponde negativamente, lasciare invariato `docs/changelog` e concludere indicando che è stata prodotta soltanto la documentazione.

### 6. Scrivere il changelog dopo conferma

Confermata la chiusura, creare `docs/changelog` se non esiste e scrivere `docs/changelog/{branchname}-docs.md`.

Per il contenuto applicare la skill `keep-a-changelog-from-diff`: analizzare il confronto con `main`, usare la convenzione Keep a Changelog e riportare solo cambiamenti supportati dal diff e dalla documentazione letta. Il file deve essere in italiano, con questa forma:

```markdown
# Changelog: {branchname}

Confronto: `main...{branchname oppure commit-end}`
Stato: Branch chiuso.

## Added

## Changed

## Deprecated

## Removed

## Fixed

## Security
```

Mantenere soltanto le sezioni che contengono modifiche effettive; non inserire sezioni vuote, placeholder, ipotesi o voci dedotte dai soli nomi dei file. Usare `Added` per funzionalità nuove, `Changed` per comportamenti o componenti modificati, `Deprecated` per elementi ancora disponibili ma marcati per la dismissione, `Removed` per elementi eliminati, `Fixed` per correzioni e `Security` per modifiche di sicurezza. Una stessa modifica non deve essere duplicata in più sezioni.

Se il changelog esiste già, leggerlo e chiedere conferma prima di aggiornarlo. Non sovrascriverlo senza consenso.

## Regole vincolanti

- Tutto il testo generato deve essere in italiano, ad eccezione dei nomi tecnici, dei comandi Git e delle intestazioni standard `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed` e `Security` richieste da Keep a Changelog.
- La documentazione deve descrivere ciò che il branch o l'intervallo contiene realmente, non ciò che avrebbe dovuto contenere secondo la spec.
- Non inventare `{specname}`, `{branchname}`, requisiti soddisfatti, test superati, utenti, impatti o assenza di criticità.
- Non includere modifiche locali non committate nel perimetro; segnalarle nella documentazione se sono state rilevate.
- Non modificare codice, test, spec, intent, recap o altri documenti oltre ai due output previsti.
- Non eseguire `git add`, `git commit`, `git push`, comandi `gh` o altre operazioni remote.
- Non creare il changelog se l'utente non ha confermato che il branch è chiuso.
- Se fonti del repository sono in conflitto, segnalarlo e chiedere all'utente; non risolverlo tacitamente.
- Se il diff non consente di stabilire un fatto, dichiarare l'informazione non determinabile invece di colmare il vuoto con un'ipotesi.

## Verifica finale

Prima di concludere:

1. Controllare che il documento di implementazione sia nel percorso esatto `docs/documentation/{specname}-docs.md`.
2. Se l'utente ha confermato la chiusura, controllare che il changelog sia nel percorso esatto `docs/changelog/{branchname}-docs.md` e che usi solo categorie Keep a Changelog non vuote.
3. Verificare che i documenti riportino il perimetro analizzato e distinguano fatti, verifiche e attività residue.
4. Rileggere i diff dei documenti generati per intercettare placeholder irrisolti, nomi non normalizzati, claim senza evidenza e sezioni vuote non ammesse.
5. Riassumere i percorsi creati o aggiornati e indicare se il changelog è stato omesso perché l'utente non ha confermato la chiusura.

## Errori comuni

- Confrontare un branch con `main` usando un intervallo che omette la storia divergente o include modifiche non pertinenti.
- Interpretare `commit-start` come esclusivo quando l'utente ha fornito una coppia che delimita l'intervallo implementato.
- Scegliere una spec o un branch tra più candidati senza chiedere conferma.
- Scrivere un changelog prima di avere chiesto se il branch è chiuso.
- Riempire il changelog con categorie vuote o con voci basate soltanto sui nomi dei file.
- Presentare test presenti nel diff come test eseguiti.
- Sovrascrivere documenti esistenti senza consenso.
- Creare commit o modifiche remote mentre si producono i documenti.
