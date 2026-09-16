# Lasa Coding Skills

Una raccolta di skill riutilizzabili per AI coding agent. Le skill aiutano a trasformare un'idea in una specifica, una issue implementabile e un'implementazione verificata, mantenendo la tracciabilità fino alla documentazione finale.

Le istruzioni sono principalmente in italiano e sono distribuite nel formato `SKILL.md`, compatibile con gli agenti che supportano l'Agent Skills ecosystem.

## Skill disponibili

| Skill | Scopo |
| --- | --- |
| [`capture-intent`](capture-intent/SKILL.md) | Trasforma appunti e requisiti grezzi in un intent Markdown stabile, senza inventare requisiti. |
| [`intent-to-spec`](intent-to-spec/SKILL.md) | Trasforma un intent in una specifica di requisiti e progettazione integrabile nella codebase. |
| [`spec-to-issue`](spec-to-issue/SKILL.md) | Prepara una proposta di issue GitHub a partire da una specifica, preservando requisiti e tracciabilità. |
| [`implement-issue`](implement-issue/SKILL.md) | Implementa una issue nella codebase esistente applicando un flusso TDD e verificando i criteri di accettazione. |
| [`review-implementation`](review-implementation/SKILL.md) | Esegue una code review dell'implementazione rispetto a issue, spec, criteri e standard della codebase. |
| [`branch-to-docs`](branch-to-docs/SKILL.md) | Documenta il risultato di un branch e, dopo conferma, prepara il changelog rispetto a `main`. |
| [`keep-a-changelog-from-diff`](keep-a-changelog-from-diff/SKILL.md) | Produce o aggiorna changelog in italiano a partire da diff, commit o intervalli di release. |
| [`validate-traceability`](validate-traceability/SKILL.md) | Controlla la coerenza della catena intent → spec → issue → implementazione → documentazione. |
| [`close-issue`](close-issue/SKILL.md) | Prepara un Conventional Commit aggregato e i commenti di chiusura delle issue, senza eseguire operazioni remote. |

## Flusso consigliato

```text
capture-intent
      ↓
intent-to-spec
      ↓
spec-to-issue
      ↓
implement-issue
      ↓
review-implementation
      ↓
branch-to-docs
      ↓
validate-traceability
      ↓
close-issue
```

Il flusso include alcune attività esterne alle skill: pubblicare manualmente la issue su GitHub, creare il commit dopo l'implementazione e chiudere la issue dopo aver verificato il risultato. Le skill che producono documenti o report dichiarano esplicitamente i propri percorsi e non modificano automaticamente issue, commit o remote.

## Prerequisiti e convenzioni

- Le skill non richiedono dipendenze runtime: sono istruzioni Markdown.
- Le skill che consultano issue GitHub richiedono GitHub CLI (`gh`) configurata e autenticata.
- `review-implementation` e `branch-to-docs` lavorano su un perimetro Git esplicito; alcune verifiche richiedono il branch locale `main`.
- Il flusso usa convenzioni di percorso come `docs/intents`, `docs/specs`, `docs/issues`, `docs/adr`, `dev/implementation`, `docs/documentation` e `docs/changelog`.
- Prima di installare o eseguire una skill, verifica sempre le istruzioni del relativo `SKILL.md` e i comandi che l'agente potrebbe eseguire.

## Disclaimer

Queste skill sono state create per supportare il flusso di lavoro personale dell'autore. Non rappresentano un processo universale e potrebbero non essere adatte a ogni team, codebase, agente, progetto o contesto organizzativo.

Prima di utilizzarle, leggine sempre il contenuto e adattale alle tue convenzioni, ai tuoi strumenti e ai tuoi requisiti di sicurezza. Verifica in particolare i comandi Git/GitHub, i percorsi dei file, le autorizzazioni e i dati che l'agente potrebbe leggere o modificare. L'uso delle skill rimane sotto la responsabilità dell'utente: il risultato prodotto dall'agente deve essere controllato da una persona prima di essere applicato, pubblicato o usato per prendere decisioni.

Le skill vengono fornite senza garanzie di correttezza, completezza, aggiornamento o idoneità a uno scopo specifico. Non sostituiscono una code review, una verifica di sicurezza, una consulenza legale o altre valutazioni professionali quando necessarie.

## Licenza

Il progetto è distribuito con [BSD Zero Clause License (0BSD)](LICENSE). È una licenza permissiva senza obbligo di conservare attribuzione o testo della licenza nelle redistribuzioni, con esclusione di garanzie e responsabilità.
