# The Hunt — Examples for Human and Artificial Investigators

*Companion to [The Hunt](The_Hunt.md) and [AGENTS.md](../AGENTS.md).*

## Introduction

These examples are not recipes for constructing the perfect query. Nor are they exercises in using as many retrieval operators as possible.

They illustrate how an investigator might interact with an addressable information space.

Imagine entering a university research library. You wander the stacks, open books, inspect passages, follow references, discover unexpected relationships and sometimes reconsider why you came.

The process is non-monotonic. You may broaden or narrow the investigation, change its direction, abandon a hypothesis, pause and return another day.

CoreQuarry provides an instrumentarium for this activity.

The examples below distinguish demonstrated CLI operations from illustrative investigative strategies. Database paths, available structures and supported options depend on the installed collection and engine version.

---

## Example 1 — Entering unfamiliar territory

**Investigative question:** What mechanisms have been proposed for producing vaccines against viruses?

An inexperienced investigator might immediately construct a restrictive query containing every important word in the question.

But at this stage, we do not know the vocabulary of the collection.

Perhaps the documents speak of immunization, attenuation, antigens, viral vectors or immune responses. Perhaps the relevant material does not contain the word "mechanism" at all.

### Begin with orientation

Search using individual terms or permissive ranked queries.

Inspect the previews.

Learn how the material expresses the concepts that interest you.

### What might happen next?

A preview introduces the expression "live attenuated virus."

This suggests a new direction. We might investigate attenuation rather than continue searching for the original wording.

Another result introduces a terminology that we did not anticipate.

Our next query should reflect what we have learned, not merely repeat the original question with additional constraints.

### Lesson

A query is a hypothesis about how information might be expressed.

Do not confuse the vocabulary of the question with the vocabulary of the evidence.

The availability of sophisticated operators does not oblige you to use them.

---

## Example 2 — Establishing a relationship within a patent

Having acquired some familiarity with the material, we now have a more precise question.

Do the words "virus" and "vaccine" occur within the same instance of a patent claim?

This is different from asking whether a patent contains both words.

### Demonstrated operation

```bash
../bin/Isearch -d /Volumes/D50/indexes/PATENTS -show -top 2 virus vaccine AND:CLAIM
```

An observed evidence preview included:

```text
Query: "virus" "vaccine" AND:CLAIM

Evidence: [*CLAIM-TEXT*] a vaccine virus consisting
of a live avian virus ...
```

The structural constraint is the enclosing CLAIM instance, while the evidence appears within its descendant CLAIM-TEXT.

### Why the operator matters

A patent may contain "virus" in one claim and "vaccine" in another.

A document-level conjunction cannot distinguish that situation from their occurrence in the same claim instance.

A relevance score does not establish that the requested structural relationship holds.

### What should the investigator do next?

Inspect the actual claim.

Determine whether the relationship is substantively interesting, rather than merely lexically correct.

Perhaps retrieve the surrounding context. Examine related claims. Look for mechanisms that differ from the current hypothesis.

### Lesson

Exactness and relevance are different.

A structural predicate establishes a relationship under defined semantics. It does not establish the truth of a scientific proposition or the legal interpretation of a patent.

---

## Example 3 — Finding relationships without knowing the schema

Sometimes we suspect that two occurrences belong to the same meaningful structural unit, but do not know the name of that unit.

### Demonstrated operation

```bash
../bin/Isearch -d /Volumes/D50/indexes/PATENTS -show -top 2 virus software PEER
```

In the demonstrated collection, this located evidence sharing a paragraph container without explicitly naming that container in the query.

### Investigative significance

The investigator does not always need to begin with a complete understanding of the document schema.

Discovering the structure may itself be part of the investigation.

PEER and ANCESTOR express different structural relationships. Consult the operator reference before substituting one for the other.

### Lesson

The information space may contain useful structures that are initially unknown to the investigator.

An expressive retrieval language can help discover and exploit those structures.

---

## Example 4 — From a line to an entire bookshelf

Consider Shakespeare's collected works.

What constitutes a document?

A line? A speech? A scene? An act? A play? The collected works?

The answer depends on the investigation.

### Demonstrated operation

```bash
../bin/Isearch -d /tmp/BART -P speech -show to be or not
```

The displayed material includes Hamlet's containing speech:

```text
[SPEAKER] HAMLET

To be, or not to be: that is the question:
Whether 'tis nobler in the mind to suffer
...
```

We locate a familiar expression but request the containing speech.

### Continue the investigation

Having read the speech, we may become interested in:

- Its dramatic context.
- Other speeches by Hamlet.
- Related expressions elsewhere in the play.
- Similar constructions in other plays.
- Patterns across Shakespeare's collected works.

The scope of the investigation changes.

We do not necessarily want the retrieval boundary to remain fixed at the scale of the original match.

### Three separate decisions

1. Locate the evidence.
2. Assess the evidence through a useful preview.
3. Retrieve the region needed for interpretation.

The matching evidence, its preview and the retrieved region can have different boundaries.

### Beyond the record

The global coordinate space permits annotations whose extent crosses physical record boundaries.

This does not imply that adjacent records are semantically related. A book about fruit flies indexed immediately before Lolita does not acquire an intellectual relationship to it by virtue of address adjacency.

But an explicitly constituted collection, such as Shakespeare's collected works, may be a meaningful larger structure.

### Lesson

The record is not necessarily the fundamental unit of retrieval.

The investigator determines the appropriate scale.

It may be a line in a book or the entire bookshelf.

---

## Example 5 — An unsuccessful query

An investigator searches for several terms joined by AND, together with an exact phrase and a structural constraint.

The engine returns nothing.

What has been established?

Only that the particular query, under the particular execution conditions, produced no matches.

We have not established that the information does not exist.

### Possible responses

Investigate whether:

- The exact wording was an unjustified assumption.
- Different terminology is used in the collection.
- The structural field exists.
- The query was parsed as intended.
- Character encoding or tokenization affected the matching.
- The terms occur separately but not together.

Remove constraints selectively.

Try individual terms or a ranked disjunction.

Inspect the resulting evidence.

Where appropriate, use MAYBE or NARROW according to their documented fallback semantics.

### A distinction

If the original purpose was to verify an exact predicate, do not confuse the results of a relaxed query with verification of that predicate.

### Lesson

An empty result can be informative, but only if we understand what was tested.

Do not confuse failure to formulate a successful query with absence of evidence.

---

## Example 6 — Ranking is a policy

Suppose we are investigating an unfamiliar collection.

A conventional lexical ranking might provide an effective orientation.

Later, we become interested in the proximity or collective span of particular occurrences.

Later still, we want to establish whether an exact identifier occurs within a particular structural container.

These are different tasks.

### Possible investigative choices

- BM25 for conventional document-level lexical ranking.
- Positional scoring when the distribution of occurrences matters.
- Semantic retrieval when conceptual similarity or paraphrase matters.
- PROMOTE or DEMOTE when secondary evidence should influence scores.
- Exact predicates without relevance ranking when testing a specific relationship.

There is no obligation to retain the same ranking policy throughout the investigation.

### A historical design consideration

In the 2022 handbook, a problem was identified with normalizing dynamically selected retrieval regions by average record length.

A larger region may have been selected precisely because it contains more useful evidence. Penalizing its length may work against that selection.

The extent of the match, the physical record length and the extent of the context requested by the investigator are not necessarily the same quantity.

### Lesson

Ranking can prioritize attention.

It should not determine the ontology of the information space or permanently dictate the direction of the investigation.

---

## Example 7 — Changing one's mind

An investigator begins with a hypothesis H1.

Initial searches locate evidence apparently consistent with H1.

Further inspection reveals an unexpected distinction. Some sources use the same terminology in a materially different sense.

The investigator now develops an alternative hypothesis H2.

### What should happen?

Do not merely continue accumulating support for H1.

Revisit earlier material under H2.

Search for observations that would distinguish the hypotheses.

Investigate contradictory evidence.

Change the terminology, scope or retrieval policy where necessary.

Previously rejected material may become central to the revised investigation.

### The language-game

In the spirit of Wittgenstein's language-games, the meanings and uses of the concepts with which we began may become clearer or change during the investigation.

Relevance is not necessarily invariant.

### Lesson

Discovery is non-monotonic.

The investigator must be free to revise not only queries but also assumptions, interpretations and questions.

---

## Example 8 — Multiple readings of the same text

Consider a passage in a Shakespeare play.

Different annotation layers might identify:

- Its dramatic structure.
- Its grammatical structure.
- Its poetic structure.
- Its historical references.
- Its rhetorical figures.
- A scholarly interpretation.
- A competing scholarly interpretation.

These structures need not share identical boundaries or form a single hierarchy.

The investigator may be interested in the relationships between these annotations rather than merely in the words of the passage.

### A further architectural possibility

An investigator could maintain annotations representing provisional hypotheses, supporting evidence and contradictory evidence.

Such annotations could themselves become objects of further investigation.

This describes a possible investigative workflow, not a claim that every required annotation-writing or inference operation is already exposed by the current agent interface.

### Lesson

Annotations are not simply descriptive metadata.

They provide a means of representing multiple structures and interpretations over a common addressable information space.

---

## Example 9 — The unfinished hunt

After a period of investigation, we have acquired useful evidence but have not reached a satisfactory conclusion.

Some sources remain unexplored.

Two competing interpretations remain plausible.

Further investigation may require more time, another collection or additional expertise.

### Suspend rather than manufacture an answer

Preserve, where possible:

- The current question.
- The questions from which it developed.
- The working hypotheses.
- Source references and evidence coordinates.
- Observations and interpretations.
- Contradictions.
- Promising unexplored leads.
- The reason for suspending the investigation.

Return later with that state available.

A suspended investigation is not necessarily a failed investigation.

### Lesson

The investigator needs a stopping policy, not an obligation to produce an answer regardless of the state of the evidence.

---

## Example 10 — Recognizing the wrong question

Perhaps the most interesting outcome of a hunt is the realization that the original question was not the one worth asking.

This is difficult to evaluate with a benchmark that defines the information need and relevant document set in advance.

Yet it is a familiar experience in research.

The investigator discovers a relationship, contradiction or previously unrecognized distinction that changes the direction of the undertaking.

There may be no predetermined query that would have expressed the eventual information need.

The need emerged through investigation.

### Lesson

An artificial investigator should not merely be capable of finding what it has been told to find.

It should be capable of discovering what it ought to investigate next.

---

## Final observations

The examples above are not stages in a compulsory workflow.

You may begin with an exact identifier. You may begin with a semantic query. You may already know the relevant structure. You may discover that a simple search is sufficient.

The appropriate operation depends on the state of the investigation.

The retrieval algebra is an instrumentarium, not an itinerary.

A search result is not necessarily an answer. It is a lead.

Exactness is a contract. Approximation is a policy. Relevance is contextual. Discovery is a process.

**Let the hunt continue.**
