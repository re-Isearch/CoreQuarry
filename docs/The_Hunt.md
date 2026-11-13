# The Hunt

## Re-search as an investigative paradigm for agents and people

**Working conceptual note | CoreQuarry | 13 November 2026**

> **A search result is not an answer. It is a lead.**
>
> The Hunt treats searching as an iterative investigation of an addressable information space. Its central questions are not merely *Which ten items rank highest?* but *Where is the evidence? How is it related? What context is needed? What should be investigated next?*

**Scope and attribution.** This note articulates *The Hunt* as a paradigm developed in connection with CoreQuarry. CoreQuarry descriptions and CLI examples below are based on the author's account, operator reference, and observed terminal outputs; they are not independent performance or correctness certifications. Statements about published research are attributed to the cited papers. Proposed agent workflows and evaluation methods are design suggestions, not claims of already shipping features.

---

## 1. The problem with treating search as a transaction

The familiar search interaction is linear:

1. Enter a query.
2. Receive an ordered list.
3. Inspect one of the first few results.
4. Stop—or reformulate the same kind of query.

This interface has real practical advantages. Human attention is limited; simple ranked lists are efficient to scan. But it also embodies assumptions that should not be confused with universal properties of information retrieval:

- A fixed object—often a document, passage, or precomputed chunk—is the unit of retrieval.
- One relevance function can order those objects for the entire inquiry.
- Results beyond a small visible prefix are effectively unimportant.
- A short excerpt around an early match is adequate evidence for judging a result.
- The goal of retrieval is to deliver an answer rather than to enable an investigation.

The Hunt disputes the generality of those assumptions. A library is a useful analogy. A researcher entering the stacks might need a sentence, speech, chapter, book, shelf, or collection. One book reveals a footnote; the footnote suggests another author; the author suggests a different vocabulary and an entirely new question. The unit of retrieval and the criteria of relevance change *during* the work.

There is also a political-economy critique. A single ranking controls what becomes visible to the user, even if a service advertises millions of nominal results. A commercial system may monetize that control through placement and attention. The Hunt treats this as a design risk—not as a claim that every ranking algorithm was created with the same commercial intent. A ranking is a useful view; it should not become the only permissible view.

**The shift:** from *answer delivery* to *evidence navigation*; from *search once* to **re-search**.

This perspective also has a pre-agent history. In discussions linked to the Jan van Eyck Academie in Maastricht and the Metahaven project *Exodus: Cross Search*, the CoreQuarry author was already concerned with how linear ranking makes alternative paths of discovery effectively invisible. The Hunt extends that critique from a human-facing search interface to an agent-facing retrieval architecture. This lineage is based on the author's account of the project, rather than a claim that the exhibition implemented CoreQuarry's later algorithms [7].

## 2. What “The Hunt” means

The Hunt is a **revisable, evidence-driven investigative process** in which each search changes what the investigator knows and therefore changes which search should come next. The investigator may be human, an agent, or a combination.

A useful conceptual loop is:

```text
question / suspicion
       |
       v
LOCATE  -->  INSPECT  -->  RELATE  -->  REFRAME
   ^                                      |
   |                                      v
   +------- challenge / branch <----- RETRIEVE
                          |
                          v
                   assess coverage,
                   contradiction, cost
                          |
                    continue or stop
```

This is not a promise that an agent should browse indefinitely. A good hunt budgets its work, keeps provenance, searches for counterevidence, notices unexplored directions, and knows when available evidence is sufficient for the task. It can return *uncertain* or *not established* instead of forcing an answer.

### Principles

1. **The query is a hypothesis, not a verdict.** Results suggest what to test next.
2. **The data retains its addressability.** Evidence should remain tied to its source positions and structure.
3. **The retrieval unit is a search-time choice.** The match, preview, and returned context may have different boundaries.
4. **Relationships are first-class.** Coincidence in a document is not the same as occurrence in one claim, one speech, one annotated event, or one ordered relation.
5. **Exactness and relevance are separate.** An exact phrase predicate can be evaluated for correctness without assuming it solves the user's information need.
6. **Ranking is negotiable.** Different stages may need different relevance functions—or no scores.
7. **A preview is a clue, not the result itself.** Compact evidence helps decide whether to investigate further.
8. **The investigation is allowed to branch.** Contrary evidence and alternative explanations are not noise to be eliminated by Top-K.
9. **The stopping rule follows the task.** Confidence, source coverage, time, cost, and novelty matter more than an arbitrary fixed K.

## 3. Why a coordinate space is the right substrate

The technical intuition behind CoreQuarry is that textual material can be understood as **addressable content with one or more superimposed structures**.

The underlying content is laid out in a common coordinate space. Token occurrences have addresses; regions have start/end boundaries. A dramatic speech, a patent claim, a paragraph, a JSON field, a tagged annotation, and a retrieved vector-associated region can all be identified by positions or spans. This does **not** mean every value is text or every relationship is reducible to an interval. It means that heterogeneous retrieval results can point back to compatible source coordinates.

```text
source coordinate axis:   0 ------------------------------------------->

play / scene:             [---------------------------------------)
speech:                       [----------------------------)
line:                            [------)
annotation A:                    [----------------)
annotation B:                             [---------------------)
lexical occurrence:                  [--)
semantic match span:                     [-------------)
```

The overlaps need not form a properly nested tree. XML's DOM is one useful *view* of structure, not a general ontology of all textual relationships. Crossing annotations, as in TAGML or BRAT-style annotation, can be anchored to overlapping regions. Additional typed or graph relationships may need their own representations, but they do not require abandoning the shared source coordinates.

This approach is format-agnostic at the retrieval level. CoreQuarry's built-in document classes include XML, JSON, JSON-LD, NDJSON, ES bulk NDJSON, Markdown, HTML, DOCX, PDF, SGML, TAGML, BRAT, bibliographic formats, mail and more. These names cover parsers, filters and ingestion behaviors—not merely file extensions. The important point is **not** the number of formats; it is that a format handler need not dictate the fundamental retrieval algebra.

### 3.1 Not every relation is containment

Once evidence is addressable, an investigator can distinguish:

- *same record* from *same structural instance*;
- *immediately shared container* from *some common ancestor*;
- *before* from *after*;
- *adjacent* from *within a specified number of bytes*;
- *overlapping* from *nested* or *disjoint* regions;
- *exact occurrence* from *model-based similarity*.

These distinctions are useful even when no natural-language understanding is required. For a parts catalog, the physical closeness of a part number and a model number can be meaningful. For a patent, two words in the same **claim instance** are materially different from two words occurring somewhere in the same patent. For Shakespeare, the line, speech, scene and play are all legitimate scopes for different questions.

### 3.2 Intellectual foundations: Gonnet, PAT arrays, and HyTime

The Hunt and CoreQuarry build consciously on an established history of address-oriented text and hypermedia systems. **The conception of text as addressable positions and spans is not presented here as an invention of CoreQuarry.** Two intellectual debts deserve explicit, substantive credit:

- **Gaston H. Gonnet and the PAT-tree/PAT-array tradition.** Gonnet's earlier work on unstructured text databases and the PAT system for the Oxford English Dictionary helped establish position-oriented indexing as an alternative to treating text merely as documents with keywords. The later account by **Gonnet, Ricardo Baeza-Yates, and Tim Snider**, *New Indices for Text: PAT Trees and PAT Arrays* (1992), describes indexing text without requiring predefined words or document structure, while still being able to use available structure. The paper also acknowledges the independent suffix-array development by Manber and Myers [4]. This is a central technical antecedent for CoreQuarry's position-oriented lexical index, not a passing resemblance.
- **The collective development of HyTime (ISO/IEC 10744).** HyTime grew from sustained SGML and hypermedia standardization work, principally through the ANSI X3V1.8M committee and later ISO/IEC standardization. The 1992 standard and 1997 revision supplied a principled vocabulary for *location addressing*, links across information objects, and finite coordinate spaces for spatial and temporal relationships. Its addressing model could refer across element boundaries and into external entities: it was never simply a DOM-like tree [5]. This broad body of work is a central conceptual antecedent for representing multiple structures over addressable content.

A more recent architectural relative is **Charles Clarke's Cottontail / annotative indexing**, documented in 2024, where content and metadata likewise occupy intervals in a common address space [6]. Its relationship to CoreQuarry is best examined through specific operator semantics, physical indexes, and workflows. The conceptual ground was laid well before either implementation.

CoreQuarry's distinct engineering choices include its positional index, tunable fixed-width SIS comparison directory, typed sidecars, query-time structural operators, and control over retrieval scope. **The Hunt** names the accompanying investigative approach: finding leads, inspecting and relating evidence, changing the retrieval unit, and pursuing further questions rather than ending at a predetermined Top-K list. Those are the matters to compare and evaluate—not a purported invention of spans.

## 4. A query algebra for evidence relationships

Giving an agent `grep` is a major improvement over a single opaque Top-K endpoint: it can reformulate queries, inspect local context, and follow leads. But complicated relationships otherwise have to be recreated as sequences of scans, parsers, scripts and application-level joins.

CoreQuarry instead exposes explicit query operators. Examples from its operator reference include:

| Operator | Meaning relevant to the hunt |
|---|---|
| `A B AND` | Both operands match the same record. |
| `A B AND:<field>` | Operand hits lie within the same instance of the named structural field. |
| `A B PEER` | Hits share the same immediate non-root structural container, without naming its path. |
| `A B ANCESTOR` | Hits share a non-root structural ancestor; this is broader than `PEER`. |
| `A B PEERb[:n]` | Ordered same-container relation; optional `n` limits the intervening **byte** gap. |
| `A B BEFORE[:distance]` | Ordered proximity relation, not a universal theory of syntax. |
| `A WITHIN:<field>` | Restrict hits to a particular field. |
| `A B MAYBE` / `NARROW` | Intersection if possible, with differing fallback choices when the intersection is empty. |
| `A B PROMOTE` / `DEMOTE` | Adjust B's scores using evidence from A without contributing A's hit evidence. |
| `A FOCUS:<count>` | Prefer/retain stronger joint evidence from multiple query terms. |

`SIBLING` is a **unary postfix rewriting modifier** for compatible expressions; it is not a generic command to return arbitrary sibling nodes. Precise operator semantics matter, and documentation should not casually replace them with vague metaphors like “semantic relationships.”

A byte-distance operator measures **byte distance**. It may be useful for serial numbers, technical identifiers and formatted records; it is not a cross-linguistic approximation to grammatical proximity. Similarly, literal phrase matching, case-sensitive exact matching, positional slop, morphological matching, and semantic similarity are different operations and deserve different correctness tests.

### 4.1 Actual CLI example: patent claim containment

The following query was demonstrated against a patent index with 656,519 records:

```bash
../bin/Isearch -d /Volumes/D50/indexes/PATENTS -show -top 2 virus vaccine AND:CLAIM
```

The displayed evidence included:

```text
Query: "virus" "vaccine" AND:CLAIM
Evidence: [*CLAIM-TEXT*] a vaccine virus consisting of a live avian virus ...
```

The *constraint* is the enclosing `CLAIM` instance; the *evidence* appears in its descendant `CLAIM-TEXT`. The query does not require the text to be an immediate child of `CLAIM`, nor does it mistake two separate claims in one patent for one claim.

Another demonstrated query was:

```bash
../bin/Isearch -d /Volumes/D50/indexes/PATENTS -show -top 2 virus software PEER
```

This returned evidence from a paragraph (`[*P*]`) without specifying `P` in the query. In that test, `AND:P` and `PEER` returned the same top examples but had different measured runtimes; this is an illustration of semantics, **not** a performance benchmark or proof of universal equivalence.

## 5. Separate locating, assessing and retrieving

The Hunt separates at least three decisions that many RAG pipelines collapse into one chunk:

**Locate.** Where is the evidence that satisfies the current query?

**Assess.** What small, informative preview helps a human or agent judge whether this is a promising lead?

**Retrieve.** Which region(s) should be inspected now: a line, speech, claim, record, whole document, or a larger collection?

The `-show` option provides an evidence preview. CoreQuarry's described heuristic balances coverage, diversity, structural coherence and the cost of exceeding a presentation budget. Its default budget is **130**, chosen as a pragmatic attention heuristic rather than a scientific constant. It can favor a slightly larger structural unit or more query coverage if worthwhile, with damping against uncontrolled expansion. The preview is **not** the canonical retrieval boundary and is not guaranteed to be an answer.

The retrieval unit is separately selectable at search time. For example, this demonstrated query requests the whole containing speech:

```bash
../bin/Isearch -d /tmp/BART -P speech -show to be or not
```

Its output begins with Hamlet's full speech rather than only the famous line:

```text
[1] speech: <SPEAKER>HAMLET</SPEAKER>
<LINE>To be, or not to be: that is the question:</LINE>
<LINE>Whether 'tis nobler in the mind to suffer</LINE>
...
```

This is the dynamic-unit thesis in miniature: **the index locates evidence; the investigation decides how much context to retrieve.** A line-level match, preview snippet, and speech-level retrieval can be three different spans, all referring to the same source.

## 6. Ranking is a choice, not the definition of relevance

CoreQuarry exposes multiple normalization and ranking approaches, including BM25, E2 (pairwise term coherence and collective span), byte-count, logarithmic and other modes, as well as an option to omit score normalization. This does not make all rankings equally good. It makes ranking an explicit *policy* that can change with the investigative question.

For example, a hunt may switch between:

- strict identifier matching to establish a fact;
- structural containment to establish co-occurrence in one claim or case;
- a relevance normalization to triage a large candidate set;
- a different weighting or exploratory query to find neglected evidence;
- semantic retrieval where paraphrases or conceptual associations matter.

CoreQuarry also supports vector sidecars, including path/container-bound HNSW graphs, alongside other typed sidecars. The important architectural point is that a retrieved match identifies an addressable span. **The Hunt is not an argument that BM25 defeats embeddings or vice versa.** Those methods expose different relationships; neither can replace a full investigative strategy. Cross-model score calibration, where required, is a separate technical problem and should not be treated as automatically solved by shared coordinates.

### 6.1 Adaptive retrieval is not just a larger K

The Hunt's **Adaptive** ambition is to let evidence and the state of the investigation influence the next retrieval policy and stopping decision, rather than committing to a fixed number of highest-scored chunks. That might mean tightening a constraint after seeing false positives, broadening an unsuccessful conjunction, changing the evidence scope, exploring a counterexample, or halting once a claim has adequate corroboration.

This describes a paradigm, not a fully specified numerical algorithm. CoreQuarry has concrete operators such as `MAYBE` and `NARROW` with documented fallback semantics, but those operators alone should not be conflated with a complete adaptive investigation controller. Query-language expressiveness, agent planning, and stopping policy remain separate layers.

### 6.2 Exactness is not graded relevance

If a query requests an exact phrase under specified tokenization and case rules, an implementation should be judged against the **predicate's correctness**. A phrase with permitted slop is a different predicate. A benchmark's nDCG score may legitimately reward approximate matches, but that must not be relabeled as proof that exact phrase matching is less correct.

A compact formulation:

> **Exactness is a contract. Approximation is a policy. Relevance is contextual. Discovery is a process.**

## 7. From human attention limits to agent exploration

The older ranked-list interface was partly a response to human attention limits. Agents relax that bottleneck: they can run more searches, test alternative query formulations, inspect different scopes, compare explanations, and revisit discarded hypotheses.

But the constraint does **not** disappear without remainder. Agents still consume tokens, time, computation and money; they may chase noise, amplify bias, overlook permissions or endlessly repeat unproductive searches. The Hunt replaces an implicit Top-K stopping rule with an **explicit investigative budget and stopping policy**.

A useful agent trace should record:

1. the current question and competing hypotheses;
2. the exact query expression and policy choices;
3. source addresses and spans of evidence inspected;
4. why particular evidence was accepted, challenged or rejected;
5. which paths have been examined and which remain open;
6. contradictions and missing information;
7. the cost of further exploration and the stopping rationale.

A hunt can be broad without being indiscriminate, and efficient without pretending that the first ranking is a final judgment.

---

## 8. Comparison with three 2026 agentic-search papers

These papers are evidence that the research community is reexamining the fixed Top-K retrieval interface. They do **not** all make the same claim, and none should be reduced to a slogan that “grep beats embeddings.”

### 8.1 Sen et al. — *Is Grep All You Need? How Agent Harnesses Reshape Agentic Search* (May 2026) [1]

**What it studies.** A comparison of grep-style lexical retrieval and vector retrieval within different agent harnesses and tool-delivery styles. The paper uses a 116-question LongMemEval subset, a custom Chronos harness, and provider-native CLI harnesses; it contrasts inline versus file-based delivery and considers increasing irrelevant context.

**What it contributes.** Retrieval effectiveness in an agent is not a property of the retrieval method alone. Harness behavior, context management, and result presentation can change end-to-end outcomes. The reported grep advantage is conditional on the studied settings, not a universal ordering of retrieval mechanisms.

**Connection to The Hunt.** The agent is an active searcher, and the interface through which evidence arrives matters. The Hunt agrees strongly.

**Difference.** Sen et al. primarily evaluate retrieval *mode* and harness mediation. The Hunt is a broader account of investigative state, query-time retrieval granularity, explicit evidence relationships, changing relevance policies, and source-address provenance. Grep can participate in a hunt, but grep by itself does not supply CoreQuarry's structural/positional algebra.

### 8.2 Li et al. — *Beyond Semantic Similarity: Rethinking Retrieval for Agentic Search via Direct Corpus Interaction* (May 2026) [2]

**What it proposes.** Direct Corpus Interaction (DCI): an agent uses grep, reads, shell commands and lightweight scripts on the raw corpus rather than being restricted to a similarity-retrieval API. The abstract stresses exact lexical constraints, sparse clue conjunctions, local context, multi-step hypothesis refinement, no required offline index, and changing corpora.

**What it contributes.** It shifts agency from a once-for-all retriever to an investigator capable of probing the source directly. Its reported gains are task- and benchmark-dependent; they do not establish that indexing is undesirable in all settings.

**Connection to The Hunt.** This is arguably the closest *interaction philosophy*: the evidence path is exploratory and revisable; premature Top-K truncation is a limitation.

**Difference.** DCI intentionally uses general-purpose tools and no mandatory offline indexing. CoreQuarry offers a purpose-built indexed algebra over addresses, spans and containers, so a relationship such as *same claim instance* can be declared directly rather than reconstructed procedurally from file reads and scripts. The tradeoff is real: DCI is simple and tolerant of rapidly changing local files; a specialized indexed engine requires ingestion and operational maintenance but can provide stronger indexed operations on large heterogeneous collections.

### 8.3 Clarke & Smucker — *Boolean Queries Are All You Need?* (July 2026) [3]

**What it studies.** An LLM search agent, Vole, iteratively issues Boolean queries against MS MARCO V2.1 deduplicated segments in the TREC 2024 RAG Track setting. It can request snippets, next results, or full documents. The study used 86 topics, up to 100 model calls per topic, and reported nDCG@10 = 0.6863. The paper describes the experiment as exploratory and notes limitations of the evaluation collection.

**What it contributes.** It demonstrates that an LLM can construct and revise relatively expressive Boolean patterns. The underlying **Cottontail** index uses *shortest-substring ranking*: match spans over a contiguous corpus string contribute according to length and multiplicity, without requiring global term statistics or learned weights in that ranking mode.

**The important similarity.** This work is **not** merely “grep with Boolean operators.” The paper explicitly describes annotative indexing: content and metadata represented as interval annotations over a common address space, with support in the substrate for structural queries over JSON and other capabilities. Its 2024 antecedent, *Annotative Indexing*, makes this connection even clearer [6]. That is a genuine architectural parallel to CoreQuarry's coordinate-space view and deserves explicit credit.

**The defensible difference.** The **agent experiment** deliberately exposed a narrower interface—Boolean pattern queries, a 200-word snippet, next-result navigation and full-document reads—rather than the full Cottontail engine. The Hunt, as implemented and described through CoreQuarry, foregrounds a wider search-time algebra (`PEER`, `AND:CLAIM`, ordered and byte-distance relationships, relevance modes), an independently selectable retrieval scope (`-P speech`), and evidence-preview selection. Whether CoreQuarry is more expressive or efficient than the *full Cottontail system* on a given workload is a separate empirical question, not established by these examples.

### 8.4 Comparison at a glance

| Dimension | Sen et al. | Li et al. (DCI) | Clarke & Smucker (Vole/Cottontail) | The Hunt / CoreQuarry |
|---|---|---|---|---|
| Central emphasis | Harness and delivery effects | Direct iterative corpus access | Agent-written Boolean patterns and shortest-substring ranking | Investigative paradigm plus coordinate-based retrieval algebra |
| Agent can revise search | Yes | Yes | Yes | Yes; fundamental principle |
| Raw/direct inspection | Depends on harness | Central | Snippet and full-document actions | Compact preview plus separately chosen spans |
| Index required | Depends on retriever | No | Yes | Yes for the CoreQuarry implementation |
| Span/address substrate | Not central claim | Procedural file positions | **Yes: Cottontail annotative indexing** | **Yes: common textual coordinate space** |
| Explicit structural relationships | Not main subject | Agent can script them | Substrate supports annotations/structure; limited agent-facing experiment | First-class named and schema-independent operators |
| Retrieval granularity | Harness/tool-dependent | File reads and scripts | Segment/document access in experiment | Selectable at search time; e.g. `-P speech` |
| Distinctive caution | Harness-specific findings | Direct scans have costs | Strong architectural overlap; exploratory benchmark | No published head-to-head result claimed here |

**Synthesis.** All three works push beyond the assumption that a single Top-K similarity call is sufficient. The Hunt's stronger claim is not “use lexical search instead of vectors,” but **“give the investigator a revisable algebra over addressable evidence, and keep the unit and policy of retrieval under investigative control.”**

## 9. Why the benchmark itself is part of the problem

The Hunt also questions the evaluation ontology. A leaderboard must decide:

- what counts as a retrievable unit;
- which fields, structure and metadata survive corpus preparation;
- which candidates human assessors see;
- what is called “relevant”;
- whether exact predicate correctness is confused with graded usefulness;
- how results are aggregated into a single scalar.

These choices are neither necessarily malicious nor neutral. The evaluation can be rigorous *for the task it defines* while failing to measure other capabilities. A benchmark based on fixed passages cannot establish the value of selecting speech, claim or library spans at query time. A relevance-judgment pool assembled from restricted retrieval methods may miss evidence a different method discovers; incomplete judgments must be handled explicitly. A fixed-depth nDCG score tells us something useful about a ranked run, but does not measure the trajectory of an investigation.

This is a methodological position, **not** an assertion that all BEIR datasets use one BM25-only judging procedure or that all their texts were universally truncated to 512 tokens. Those claims need dataset-by-dataset documentation. Nor does it make BM25, dense retrieval, or standard rankings useless. It limits what any one scoreboard can legitimately prove.

### Better evaluation questions for a hunt

Evaluate at least two independent layers:

**Operator correctness:** exact phrases, containment within the *same instance*, order, byte distance, overlap, negative matches, and correctness of source ranges. Here mathematical predicates can be tested against explicit ground truth.

**Investigative effectiveness:** whether an agent discovers material evidence (including counterevidence), follows promising leads, identifies missing coverage, retains provenance, selects appropriate context, and produces warranted conclusions within a cost budget.

Potential measurements include newly judged evidence coverage, hypothesis diversity, contradiction detection, citation-grounded conclusion accuracy, investigation time and cost, and stopping quality. These are proposals for research rather than an established standardized metric suite.

## 10. A concrete hunt: from clue to claim to broader context

Imagine an investigator asks: *Which vaccine-related patent claims establish a particular mechanism?* A possible sequence is:

1. **Orient:** Search broadly for terminology; inspect short evidence previews to learn the vocabulary.
2. **Constrain:** Use `virus vaccine AND:CLAIM` so the evidence belongs to the same claim instance, not merely to the same patent.
3. **Inspect:** Retrieve the relevant claim and necessary parent context, retaining source coordinates.
4. **Disconfirm:** Seek claims with similar terms but different mechanisms, or where similar terms appear only in background descriptions.
5. **Broaden or change method:** Apply different terminology, a thesaurus or semantic search where appropriate; the broader investigation need not keep the same ranking policy.
6. **Assess coverage:** Have all material variations and counterexamples been investigated? Which claims remain unexamined?
7. **Conclude provisionally:** Return supported statements with exact references, explicit uncertainty, and a record of unexplored alternatives.

The first two CLI operations in this example are demonstrated CoreQuarry capabilities. The full sequence is an **illustrative agent workflow**, not a claim that a complete autonomous Hunt agent has been demonstrated.

## 11. Why this does not require a giant graph visualization

For a play or a large corpus, explicitly drawing every span and every pairwise relation would be overwhelming. Nor is it always necessary to materialize those relations as graph edges. If intervals and addresses are present, many containment, overlap and order relationships can be computed when needed.

**The query is a lens on the graph.** The investigator sees a local neighborhood, follows a relation, changes the retrieval scale, and exposes the next neighborhood. A full static visualization is no more necessary than a map of every intellectual connection in a library before taking a book from a shelf.

This is also a useful answer to the DOM mindset: a tree is one possible interpretation of content; it is not the only one. TAGML's overlapping structures offer a vivid demonstration, provided the actual parser, indexing and querying behavior are validated end to end.

## 12. Practical design consequences for an agent interface

A Hunt-oriented search API should ideally expose:

- **Declarative query expressions** with explicit semantics rather than opaque “best results” alone.
- **Stable source coordinates and provenance** so evidence can be revisited and expanded.
- **Preview and retrieval as separate operations** with tunable budgets.
- **Granularity selection at query time** rather than fixed chunk boundaries.
- **Multiple scoring policies or score-free modes** when the task requires them.
- **A trajectory log** of queries, hypotheses, inspected spans, counterevidence and costs.
- **Branching and deliberate counter-search** rather than endless local refinement around the same highly ranked material.
- **Explicit stopping criteria** and resource, privacy and access-control boundaries.

The implementation need not expose every internal knob to a language model. A small, typed tool interface can offer progressively more capable operations. What matters is that the engine does not permanently collapse the space into an immutable list of chunks and scores.

## 13. What The Hunt is—and is not

**The Hunt is:**

- an investigative paradigm, not a new leaderboard metric;
- an argument for revisable, evidence-grounded search;
- a commitment to source positions, explicit relations, and selectable retrieval scope;
- a framework in which lexical, structural, typed and semantic mechanisms may play different roles;
- a way to think about what an agent does *between* its first clue and its conclusion.

**The Hunt is not:**

- a claim that every query needs hundreds of tool calls;
- a claim that BM25 is worse than embeddings, or vice versa;
- an assertion that all graphs are just intervals;
- proof that CoreQuarry dominates Cottontail, grep-based agents, or vector search;
- a replacement for proper evaluation, source permissions or evidence verification;
- an invitation to confuse a heuristic preview with a factual answer.

## 14. A short manifesto

> We have made a habit of asking search engines for the *top results*, as though the information need came with a fixed unit of retrieval, a fixed ranking function and a fixed stopping point.
>
> But research has never worked that way. We follow clues. We open books and compare passages. We change our vocabulary, widen or narrow our scope, examine contradictions and ask new questions. Sometimes the important discovery is the question we did not know how to ask at the beginning.
>
> Agents make that style of investigation newly practical at scale. They also make the limitations of one-shot retrieval newly obvious.
>
> **The Hunt** is the proposal to build search for that process: locate evidence, preserve its coordinates, express relationships precisely, show useful previews, retrieve the context the question calls for, and let each discovery inform the next search.
>
> **Don't make the chunker decide what can be found. Don't make the ranker decide what is worth investigating. Let the hunt continue.**

---

## References and source notes

### Primary 2026 comparison papers

**[1]** Sahil Sen, Akhil Kasturi, Elias Lumer, Anmol Gulati, Vamse Kumar Subbiah. **“Is Grep All You Need? How Agent Harnesses Reshape Agentic Search.”** May 2026. [arXiv:2605.15184](https://arxiv.org/abs/2605.15184) · [HTML text](https://arxiv.org/html/2605.15184). Verified against paper text/abstract.

**[2]** Zhuofeng Li et al. **“Beyond Semantic Similarity: Rethinking Retrieval for Agentic Search via Direct Corpus Interaction.”** May 2026. [arXiv:2605.05242](https://arxiv.org/abs/2605.05242). Comparison is based on the accessible abstract and metadata; do not read it as a section-by-section technical audit of the full paper.

**[3]** Charles L. A. Clarke and Mark D. Smucker. **“Boolean Queries Are All You Need?”** July 2026. [arXiv:2607.11362](https://arxiv.org/abs/2607.11362) · [HTML text](https://arxiv.org/html/2607.11362). Verified against paper text; includes Cottontail's interval-based substrate, Vole's restricted experiment interface, and stated evaluation limitations.

### Historical and architectural context

**[4]** Gaston H. Gonnet, Ricardo A. Baeza-Yates, Tim Snider. **“New Indices for Text: PAT Trees and PAT Arrays.”** In *Information Retrieval: Data Structures & Algorithms*, 1992, pp. 66–82. [Original chapter PDF](https://www.cs.jhu.edu/~yarowsky/cs466slides/4bslides/601.466.pattrees.pdf) · [Bibliographic record](https://dblp.org/db/books/collections/FrakesB92.html). The chapter traces Gonnet’s earlier PAT work and credits independent suffix-array work by Manber and Myers.

**[5]** **HyTime** (Hypermedia/Time-based Structuring Language), ISO/IEC 10744:1992, revised as ISO/IEC 10744:1997. [ISO standard record](https://www.iso.org/standard/29303.html) · [RFC 1614 historical description](https://datatracker.ietf.org/doc/html/rfc1614) (ANSI committee work, location addressing, finite coordinate spaces).

**[6]** Charles L. A. Clarke. **“Annotative Indexing.”** 2024. [arXiv:2411.06256](https://arxiv.org/abs/2411.06256). Important parallel prior/independent work on intervals over a common address space.

**[7]** Metahaven, **“Exodus: Cross Search”**, ISEA archival record: [project link](https://www.isea-symposium-archives.org/art-events/metahaven-exodus-cross-search/). The connections to re-search and the Jan van Eyck Academie conversations reflect the CoreQuarry author's account; they are not assertions inferred from the archival webpage.

### Source of CoreQuarry examples

- CoreQuarry CLI outputs and architecture explanations shared by the author in the working discussion (Shakespeare/BART, patents, Wikisource/JSON, search-time presentation scope, operator definitions, SIS cache and sidecars).
- The supplied CoreQuarry operator reference (53 listed operators) was used to avoid inventing semantics for `PEER`, `AND:<field>`, `SIBLING`, ordering, and byte proximity.
- No standardized head-to-head evaluation of The Hunt against the three papers' systems is claimed in this document.
