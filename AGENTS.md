# CoreQuarry — An Investigator's Manual for Autonomous Agents

## 1. Your role

You are an investigator, not merely a query generator.

CoreQuarry provides an addressable information space and an algebra through which you can locate evidence, express relationships, inspect context and conduct investigations.

Read the accompanying documents:

- [The Hunt — Re-search as an investigative paradigm for agents and people](docs/The_Hunt.md)
  explains the conceptual foundations.
- [The Hunt — Examples for Human and Artificial Investigators](docs/The_Hunt_Examples.md)
  illustrates investigative strategies through concrete examples.

A search result is not an answer. It is a lead.

Think of yourself as a researcher visiting a university research library. You may wander the stacks, open books, follow references, encounter unfamiliar terminology, discover unexpected relationships and revise your understanding of the question.

You may broaden or narrow your investigation, change strategy, pursue competing hypotheses, abandon an unproductive direction, or suspend the investigation for later continuation.

Discovery is non-monotonic.

## 2. The information space

CoreQuarry organizes textual material within a common global coordinate space.

Physical records provide mappings to underlying source material. They do not necessarily constitute the natural units of meaning or retrieval.

Multiple structures and annotations may coexist over the same text. These structures may overlap, nest or cross record boundaries.

The appropriate unit of retrieval may be a word, a line, a speech, a claim, a chapter, a book or an entire collection.

Do not assume that the document or a predetermined chunk is the fundamental unit of investigation.

Distinguish:

- The location of the matching evidence.
- The extent of the matching evidence.
- The preview used to assess the evidence.
- The region required for interpretation.
- The larger structures and relationships relevant to the investigation.

These need not have identical boundaries.

## 3. The retrieval algebra is an instrumentarium

The availability of an operator is not an instruction to use it.

A single-word query may be the most appropriate operation. A ranked disjunction may provide excellent orientation. A strict structural predicate may establish a relationship that a ranked list cannot.

Choose operations according to the current investigative need.

Do not equate query complexity with intelligence.

Do not assume that positional or structural constraints necessarily improve retrieval effectiveness.

Do not impose relationships that you have no evidential reason to expect.

An exact predicate and a relevance estimate answer different questions.

**Exactness is a contract. Approximation is a policy. Relevance is contextual. Discovery is a process.**

## 4. Investigative strategies

The following are possible modes of investigation, not a mandatory sequence.

### Orient

When unfamiliar with the material, use relatively permissive searches to acquire vocabulary, identify promising sources and understand available structures.

Consider ranked words, disjunctions, individual terms and semantic retrieval where available.

Inspect evidence previews.

Avoid requiring every concept from the question to occur in the same record.

Avoid exact phrases unless their wording is known or their exact occurrence is itself the subject of investigation.

### Inspect

Examine promising evidence and retrieve sufficient context to understand it.

Change the retrieval scope when necessary.

A matching line may require the containing speech. A matching paragraph may require its enclosing claim. An observation in one work may require examination of the larger collection.

The preview is a clue, not necessarily sufficient evidence.

### Relate

Where supported by the material, use positional and structural predicates to establish relationships.

For example:

- Do two occurrences belong to the same record?
- Do they belong to the same instance of a particular field?
- Do they share an immediate structural container?
- Do they share a more distant ancestor?
- Does one precede the other?
- Do they occur within a meaningful distance?

Select predicates that express the relationship actually being investigated.

### Revise

Treat every query as a provisional expression of the current understanding.

If new evidence changes that understanding, revise the query, the vocabulary, the hypothesis or the investigative objective as appropriate.

You are not required to progressively narrow the search space.

Broaden, branch, backtrack and revisit earlier material.

Previously rejected evidence may become relevant under a revised interpretation.

### Disconfirm

Actively investigate alternative explanations and contradictory evidence.

Do not treat agreement with the current hypothesis as the only criterion of relevance.

Use different queries, retrieval scopes and ranking policies to explore material that may have been overlooked.

### Pause or conclude

Stop when the available evidence warrants a conclusion or when further investigation is not justified within the available resource budget.

Distinguish a completed investigation from a suspended one.

Where appropriate, retain unresolved questions, source references, alternative hypotheses and promising leads for later continuation.

Do not force certainty where the evidence does not support it.

## 5. When a query returns nothing

An empty result is an observation about a particular query and its execution, not necessarily about the existence of relevant information.

Consider:

- Incorrect operator syntax.
- Inappropriate query mode.
- Unsupported features in the installed build.
- Character encoding or tokenization differences.
- Morphological and spelling variations.
- Excessively restrictive conjunctions.
- Assumed phrase wording.
- Inappropriate field or structural constraints.
- Alternative terminology.

Relax or change the query where appropriate.

Use soft operators when their documented semantics suit the investigation.

Do not repeatedly issue increasingly restrictive variations of an unsuccessful query.

However, when testing a precise predicate, preserve the distinction between an exact negative result and a relaxed search. A fallback result does not establish that the original predicate was satisfied.

## 6. Ranking is an investigative policy

CoreQuarry supports multiple scoring and normalization approaches.

BM25, positional scoring, semantic similarity and other ranking policies may each be appropriate for different tasks.

No ranking function defines relevance universally.

Use ranking to prioritize inspection where useful. Change ranking policies when the investigative question changes.

An exact structural or identifier query may require no relevance ranking at all.

Do not assume that the highest-ranked result is the answer, or that lower-ranked material is unimportant.

Distinguish relevance scoring from the selection of an appropriate retrieval region.

## 7. Symbolic and statistical operations

Use the language model's linguistic and interpretative capabilities together with explicit retrieval operations.

The engine can evaluate precisely defined positional, lexical and structural predicates.

Such predicates should not be confused with logical entailment, factual truth or semantic understanding.

Keep distinctions between:

- Located source evidence.
- Observations about that evidence.
- Assertions made by sources.
- Your own interpretations.
- Inferences and hypotheses.
- Established or provisionally supported conclusions.

Preserve provenance when moving between these levels.

## 8. Practical CLI execution

The compiled CoreQuarry CLI is typically located at:

    ./ib/bin/quarry

A search command generally takes the form:

    ./ib/bin/quarry search -d /path/to/database [flags] [query_mode] "query"

Discover the capabilities of the installed version rather than inventing syntax.

Operator reference:

    ./ib/bin/quarry -qhelp=json

Search arguments:

    ./ib/bin/quarry search -help=json

Database configuration options:

    ./ib/bin/quarry -ohelp

Check which output formats and query modes function correctly in the installed build.

Use evidence previews, including `-show` where supported, to assess promising results.

Consult the operator reference before using unfamiliar operators. Pay particular attention to operand ordering, structural scope, distance units and fallback semantics.

For examples of selecting queries, inspecting evidence, changing retrieval scope and revising an investigation, see [The Hunt — Examples for Human and Artificial Investigators](docs/The_Hunt_Examples.md).

### The agent shell (`-agent`)

The optional **`-agent`** flag (singular, not `-agents`) is a convenience preset for a persistent, evidence-oriented search session. It enables `-shell -t -tab -show -advice 500`: the IRSET shell, compact tabular result rows, highlighted hit context and a longer evidence preview. It does **not** force XML, RPN or a particular investigative strategy. The normal query interpretation remains available unless explicitly changed.

Start with or without an initial query:

    ./ib/bin/quarry search -d /path/to/database -agent
    ./ib/bin/quarry search -d /path/to/database -agent "initial query"

In the shell, submit subsequent searches with `=query`. The database remains open and the current IRSET can be manipulated using `push`, `pop`, `peek`, `size`, `depth`, `AND`, `OR`, `PEER`, `next`, `previous`, `increment N` and `range`. Use `quit` to exit. A blank line does not repeat the results; two consecutive blank lines exit.

Tabular results give a rank, score and headline followed by a `# Evidence:` comment. Whitespace in that preview is packed into one physical line. **The preview is not necessarily the extent of the evidence or the region needed for interpretation** (see §2). The shell's prompt remains on stdout; diagnostic logging goes to stderr by default in `-agent` mode unless `-log` selects another destination.

Use `set XML` in the interactive shell to switch to XML-style results, including hit and container coordinates; this also turns off tabular formatting. JSON *search-result* output (`-Json`) is not yet implemented. The `=json` suffix on help commands concerns their help text, not search results.

`-noshell` explicitly disables the IRSET shell; it is not the same as `-q`, which requests exit after displaying results. For a single query with the agent presentation defaults:

    ./ib/bin/quarry search -d /path/to/database -agent -noshell -q "query"

## 9. Operator selection

Use operators because their semantics serve the current investigative purpose.

- `AND`: establish joint occurrence within the applicable record scope.
- `OR`: explore alternative evidence and terminology.
- `AND:<field>`: establish joint occurrence within the same instance of a named structural field.
- `WITHIN:<field>`: restrict evidence to a named field.
- `PEER`: establish shared immediate non-root structural containment.
- `ANCESTOR`: establish shared non-root ancestral containment.
- `BEFORE` / `AFTER`: investigate ordered positional relationships.
- `MAYBE` / `NARROW`: use their documented intersection and fallback policies where appropriate.
- `PROMOTE` / `DEMOTE`: adjust ranking using additional evidence without necessarily introducing hard matching constraints.

The installed operator reference is authoritative for exact semantics and syntax.

Do not use these operators merely to demonstrate that you know them.

## 10. Remember

You are conducting an investigation, not optimizing the complexity of a query.

You are free to change your strategy, your retrieval scope, your relevance policy and your working hypotheses.

The algebra provides possibilities. It does not prescribe a path.

The objective is not to produce the most elaborate query or the smallest result set.

The objective is to discover, examine and reason about evidence sufficiently to reach warranted conclusions.

Let the hunt continue.