---
name: close-issue
description: Usare quando una sessione di implementazione è conclusa e sono disponibili uno o più recap per preparare in chat un messaggio Conventional Commit e i commenti di chiusura delle issue GitHub.
---

# Preparare commit e chiusura issue

## Scopo

Trasformare uno o più file `{issue}-implementation-recap.md` in testo pronto da copiare: un unico messaggio Conventional Commit aggregato e un commento esaustivo per ogni issue coinvolta.

La skill produce esclusivamente testo in chat. Non crea file, commit, issue, commenti GitHub o modifiche remote e non usa `gh` per eseguire queste operazioni.

## Quando usarla

- La sessione di implementazione è terminata.
- Sono disponibili uno o più file in `dev/implementation/{specname}/{issue}-implementation-recap.md`.
- Servono il messaggio del commit e il commento per chiudere le issue senza scriverli manualmente.

## Procedura

1. Leggere per intero ogni recap indicato. Se un file non è raggiungibile, fermarsi e segnalarlo.
2. Ricavare da nome e contenuto i riferimenti alle issue GitHub e i riferimenti ai commit. Normalizzare un numero issue come `#12` e mantenere gli hash dei commit nella forma breve o completa fornita.
3. Aggregare i recap senza perdere la distinzione tra issue. Eliminare duplicati, mantenere le evidenze di test e riportare esplicitamente criticità, limitazioni e lavori residui.
4. Se manca il riferimento all'issue, chiederlo senza inventare valori. Se i recap indicano test falliti, criteri non soddisfatti o blocchi aperti, non dichiarare l'implementazione completata e non redigere un falso commento di chiusura.
5. Preparare subito un solo messaggio Conventional Commit per l'insieme dei recap. Il titolo deve seguire `<tipo>(<ambito>): <descrizione> (#issue...)` e contenere tutti i riferimenti issue pertinenti, senza link. Scegliere tipo e ambito in base alle modifiche effettive e alle convenzioni del repository.
6. Se manca l'hash del commit, consegnare soltanto il messaggio Conventional Commit e indicare che il commento di chiusura sarà preparato dopo aver ricevuto l'hash. Non chiedere l'hash prima di aver prodotto il messaggio.
7. Dopo che l'utente fornisce l'hash, preparare un commento separato per ogni issue. Il commento deve contenere almeno un riferimento al commit pertinente, senza link, e riassumere modifiche, verifiche, criteri soddisfatti e criticità residue.
8. Presentare in chat soltanto i blocchi copiabili, con etichette chiare. Non creare né modificare alcun file o servizio esterno.

## Regole vincolanti

- Usare solo informazioni contenute nei recap o già esplicitamente fornite dall'utente. Non inventare tipo Conventional Commit, issue, commit, test, criteri soddisfatti o assenza di criticità.
- Il messaggio Conventional Commit precede sempre la richiesta dell'hash: l'hash è necessario solo per il commento di chiusura.
- Un unico commit può riferire più issue: inserirle tutte nel titolo, per esempio `(#12, #15)`. Se un issue ha più commit pertinenti, elencare tutti gli hash nel relativo commento.
- Non confondere il riferimento all'issue con quello al commit: `#12` identifica l'issue, `12abcde34` identifica il commit.
- Il corpo del commit deve essere esaustivo ma basato su fatti: contesto, modifiche, verifiche, criteri soddisfatti e `BREAKING CHANGE` solo se documentato.
- Il commento di chiusura deve dichiarare il completamento soltanto quando il recap documenta verifiche concluse e criteri soddisfatti. Altrimenti produrre una nota di blocco, non un testo di chiusura.
- Non aggiungere `Closes #12` come sostituto del riferimento richiesto nel titolo del commit o del commit nel commento.
- Il messaggio conventional commit deve essere in inglese, mentre il commento di chiusura issue deve essere in italiano

## Formato dell'output in chat

### Messaggio Conventional Commit

Produrre un blocco copiabile con questa forma:

```text
feat(ambito): descrizione del risultato (#12, #15)

Contesto:
...

Modifiche:
- ...

Verifiche:
- `comando o controllo`: superato

Criteri soddisfatti:
- ...

Criticità e note:
- ...
```

Usare il tipo corretto (`feat`, `fix`, `refactor`, `docs`, `test`, `chore` o altro tipo già adottato dal repository). Omettere sezioni senza evidenze, salvo mantenere una nota esplicita quando la loro assenza è rilevante. Aggiungere `BREAKING CHANGE:` solo se il recap lo documenta.

### Commento di chiusura

Produrre un blocco separato per ogni issue:

```markdown
Implementazione completata.

Commit: 12abcde34

Sintesi:
- ...

Modifiche principali:
- ...

Verifiche:
- `comando o controllo`: superato

Criteri di accettazione:
- ...

Criticità e note residue:
- Nessuna.
```

Sostituire `Nessuna.` solo quando il recap dimostra che non ci sono criticità residue. Il commento non deve contenere link né istruzioni per eseguire la chiusura.

## Errori comuni

- Generare un commento di chiusura quando i test falliscono o mancano criteri verificati.
- Usare un numero issue come se fosse un hash commit.
- Produrre un solo commento quando i recap riguardano issue diverse.
- Riassumere genericamente senza riportare test, criteri e criticità del recap.
- Creare il file recap, il commit o la commentistica GitHub invece di restituire testo in chat.
- Generare il messaggio conventional commit in italiano
- Generare il commento di chiusura issue in inglese