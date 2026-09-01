# AGENTS.md

## 1. Purpose

This Obsidian Vault is a structured knowledge base built from source materials such as:

- Books
- PDF-to-Markdown documents
- Technical documentation
- Research papers
- Course materials
- Standards
- Specifications
- Notes

The goal is **not merely to summarize source material**.

The primary goal is to transform linear source material into a reusable knowledge network consisting of:

1. Source Notes
2. Concepts
3. Relationships
4. Knowledge Maps
5. Questions
6. Cross-unit connections
7. Cross-source connections

The knowledge base should emphasize:

- prerequisite
- dependency
- cause-effect
- problem-solution
- leads-to
- contrast
- cross-unit relationships
- cross-source relationships

Codex performs the mechanical extraction and organization.

The user remains responsible for final understanding and validation of important relationships.

---

# 2. Vault Structure

Use the following default directory structure:

```text
/
├── 00-Source/
├── 01-Units/
├── 02-Source-Notes/
├── 03-Concepts/
├── 04-Maps/
├── 05-Questions/
├── 06_review/
└── AGENTS.md
```

## 00-Source

Original source materials or converted Markdown.

These files are treated as source-of-truth material.

Do not modify them unless explicitly requested.

Recommended structure when multiple books or source collections exist:

```text
00-Source/
├── Book-A/
│   ├── Chapter-01.md
│   ├── Chapter-02.md
│   └── Chapter-03.md
└── Book-B/
    ├── Chapter-01.md
    └── Chapter-02.md
```

---

## 01-Units

`01-Units/` defines the logical processing units used for knowledge extraction.

A Unit is a **logical knowledge-processing boundary**, not necessarily one physical document.

A Unit may contain or reference:

- one source document
- multiple source documents
- multiple chapters
- multiple sections
- a selected range of chapters
- several related technical documents
- a group of documents covering one logical topic

Therefore:

```text
1 Unit
≠
necessarily 1 document
```

Instead:

```text
1 Unit
=
1 logical knowledge scope
=
1 or multiple source documents
```

### Single-document Unit

Example:

```text
00-Source/
└── Chapter-05.md
```

A Unit may represent:

```text
Unit-05
→ Chapter-05.md
```

### Multiple-document Unit

Example:

```text
00-Source/
├── Chapter-01.md
├── Chapter-02.md
├── Chapter-03.md
├── Chapter-04.md
└── Chapter-05.md
```

A Unit may be defined as:

```text
Unit-Network-Fundamentals
├── Chapter-01.md
├── Chapter-02.md
└── Chapter-03.md
```

Codex must treat all referenced documents as one logical analysis scope.

The purpose is to identify knowledge that may span document boundaries, including:

- shared Concepts
- prerequisite chains
- repeated terminology
- cause-effect relationships
- continuation of an idea
- cross-document dependencies
- contradictions
- concept evolution

### Recommended Unit definition

Example file:

```text
01-Units/Unit-Network-Fundamentals.md
```

Example content:

```markdown
# Unit: Network Fundamentals

## Sources

- [[../00-Source/Chapter-01]]
- [[../00-Source/Chapter-02]]
- [[../00-Source/Chapter-03]]

## Processing Goal

Treat all listed source documents as one logical knowledge unit.

Focus on:

- common Concepts
- prerequisite relationships
- concepts introduced in one document and reused in another
- concepts that evolve across documents
- cross-document dependencies
- important differences or contradictions
```

### Overlapping Units

The same source document may belong to more than one Unit when useful.

Example:

```text
Chapter-03.md
```

may belong to:

```text
Unit-Network-Access
```

and:

```text
Unit-Layer2-Troubleshooting
```

Do not duplicate the source file. Reuse the same source reference.

### Unit naming

Prefer meaningful topic-based names:

```text
Unit-Network-Fundamentals
Unit-Layer2-Switching
Unit-Routing-Basics
Unit-Network-Security
```

Numeric names such as `Unit-01` are acceptable, but do not assume a Unit equals one chapter.

---

## 02-Source-Notes

Structured notes generated from Units.

A Source Note explains:

- what the Unit is trying to teach
- which concepts matter
- why those concepts matter
- how those concepts relate to existing knowledge

A Source Note is **not merely a summary**.

For a multi-document Unit, create a Unit-level Source Note such as:

```text
02-Source-Notes/Unit-Network-Fundamentals.md
```

Optional per-document Source Notes may also be created when useful, but the Unit-level note should focus on the combined knowledge structure rather than concatenating summaries.

---

## 03-Concepts

Reusable atomic knowledge notes.

Examples:

```text
VLAN.md
STP.md
Routing-Table.md
REST-API.md
Observer-Pattern.md
Stoicism.md
```

Concept Notes should represent knowledge that can be referenced from multiple Source Notes.

Do not create a Concept Note for every noun or term appearing in the source.

---

## 04-Maps

Knowledge Maps and Maps of Content.

Maps organize concepts by relationships rather than by original source order.

Important relationships discovered during Unit processing must be recorded in `04-Maps/`.

In particular, the content later reported under `Relationships Added` must not exist only in the final response. It should be written into one or more relevant Knowledge Map files, such as:

```text
04-Maps/Unit-Network-Fundamentals-Knowledge-Map.md
04-Maps/Unit-Network-Fundamentals-Relationships.md
```

The final report may summarize these relationships, but the durable record belongs in `04-Maps/`.

Examples:

```text
Ethernet-Knowledge-Map.md
Routing-Knowledge-Map.md
AI-Agent-Architecture-Map.md
Book-Concept-Map.md
```

---

## 05-Questions

Questions used for:

- understanding
- review
- self-testing
- identifying knowledge gaps
- cross-concept reasoning

Questions should favor understanding over memorization.

---

## 06_review

Review notes contain uncertainty, unresolved decisions, weakly supported relationships, possible duplicate concepts, source conflicts, and items requiring human validation.

Any content identified as `REVIEW` during Unit processing must be written to `06_review/` rather than existing only in the final response or being scattered across generated notes.

Recommended file names:

```text
06_review/Unit-Network-Fundamentals-REVIEW.md
06_review/Ethernet-REVIEW.md
```

Generated Source Notes, Concept Notes, Maps, and Questions may include a short `REVIEW` section, but it should link to the relevant durable review note in `06_review/`.

# 3. Source Protection Rules

Source files are evidence.

Default behavior:

```text
READ source
       ↓
ANALYZE source
       ↓
CREATE / UPDATE notes
```

Never modify original source files unless explicitly requested.

Rules:

1. Do not alter original source wording.
2. Do not silently correct technical statements in source files.
3. Do not delete source content.
4. Do not replace source material with summaries.
5. Keep AI-generated knowledge separate from original source material.

---

# 4. Default Unit Processing Workflow

When asked to process a Unit, follow this order:

```text
1. Read Unit definition
       ↓
2. Resolve all referenced source documents
       ↓
3. Read all relevant sources
       ↓
4. Identify the Unit's core question
       ↓
5. Extract important Concepts
       ↓
6. Search existing Concepts
       ↓
7. Generate / update Source Note
       ↓
8. Create / update Concept Notes
       ↓
9. Extract relationships
       ↓
10. Build / update Maps
       ↓
11. Generate Questions
       ↓
12. Find cross-document and cross-unit relationships
       ↓
13. Mark uncertainty as REVIEW
       ↓
14. Produce execution report
```

For multi-document Units, do not analyze only the first document.

Analyze both:

- within-document relationships
- cross-document relationships

---

# 5. Generate a Source Note

For each processed Unit, create or update a corresponding Source Note under:

```text
02-Source-Notes/
```

Recommended structure:

```markdown
# <Unit Title>

## Sources

- [[Source A]]
- [[Source B]]

## Core Question

What problem, question, or subject is this Unit primarily addressing?

## Key Ideas

- ...

## Core Concepts

- [[Concept A]]
- [[Concept B]]
- [[Concept C]]

## Important Definitions

...

## Why These Concepts Exist

...

## Knowledge Progression

[[Concept A]]
↓
[[Concept B]]
↓
[[Concept C]]

## Cause and Effect

...

## Prerequisites

...

## Depends On

...

## Leads To

...

## Relationships

...

## Contrast

...

## Cross-Document Relationships

...

## Cross-Unit Relationships

...

## Important Commands / Examples

...

## Common Confusions

...

## Questions

...

## REVIEW

...
```

Not every heading must contain content.

Do not invent content simply to fill a section.

For multi-document Units, avoid producing:

```text
Summary of Document 1
+
Summary of Document 2
+
Summary of Document 3
```

as the main result.

Prefer a combined knowledge structure.

---

# 6. Concept Extraction

Identify concepts important enough to participate in the long-term knowledge base.

A Concept is usually worth creating when one or more of the following are true:

1. It appears across multiple Units or documents.
2. Other concepts depend on it.
3. It is an important prerequisite.
4. It explains an important mechanism.
5. It represents an important problem or solution.
6. It participates in several knowledge relationships.
7. It is central to understanding the broader subject.
8. It is likely to be referenced later.

Do NOT automatically create Concept Notes for:

- every technical term
- every command
- every person
- every product name
- every minor example
- every abbreviation

Avoid concept fragmentation.

Before creating a Concept, ask:

> Will this Concept probably be referenced again?

If no, leave it inside the Source Note unless there is another strong reason to make it standalone.

Prefer:

```text
30 useful Concepts
```

over:

```text
300 fragmented notes
```

---

# 7. Existing Concept Check

Before creating a Concept Note, search:

```text
03-Concepts/
```

Check for:

- exact name
- synonyms
- abbreviations
- alternate spelling
- singular/plural forms
- related terminology

Example:

```text
Spanning Tree Protocol
STP
Spanning-Tree
```

should normally refer to one Concept Note.

Do not create:

```text
STP.md
Spanning-Tree.md
Spanning-Tree-Protocol.md
```

for the same concept.

Use one canonical Concept Note.

When the same Concept appears in several documents, reuse the same Concept Note and record all relevant source references.

---

# 8. Concept Note Structure

When creating a new Concept Note, use approximately:

```markdown
# Concept Name

## Aliases

- ...

## Definition

Concise explanation of the concept.

## Why It Exists

What problem does this concept solve?

## Prerequisites

- [[Concept]]

## Depends On

- [[Concept]]

## Related Concepts

- [[Concept]]

## Leads To

- [[Concept]]

## Contrast With

- [[Concept]]

## Mechanism

How does it work?

## Source Figures

When useful, include explanatory images copied or linked from `00-Source/`.

Use Markdown image syntax and add a caption immediately below each image.

The caption must identify the source chapter and, when available, the original figure number or caption.

Example:

```markdown
![](../00-Source/images/example.jpg)
*Source: [[../00-Source/Chapter-03]], Figure 3.2 — 8P8C ports and connector.*
```

Do not add images merely for decoration. Prefer images that clarify topology, device role, cable behavior, packet flow, encapsulation, configuration structure, or troubleshooting logic.

## Appears In

- [[Source Note]]

## Questions

- ...

## REVIEW

- ...
```

Only include useful sections.

Do not create empty sections simply for template completeness.

### Concept images and captions

Concept Notes in `03-Concepts/` should include source figures whenever the source material provides a useful explanatory image for that concept.

Rules:

1. Use images from `00-Source/` or its `images/` subfolder.
2. Preserve source traceability by adding a caption directly below the image.
3. The caption must state which Chapter the image came from, and should include the original figure number/caption when available.
4. Use Obsidian-compatible Markdown image syntax.
5. Do not alter source images.
6. Do not add low-value images that do not improve understanding.

Recommended caption format:

```markdown
![](../00-Source/images/<image-file>.jpg)
*Source: [[../00-Source/Chapter N - Chapter Name]], Figure N.N — Original or concise caption.*
```

---

# 9. Concept Relationships

Relationship extraction is more important than summary generation.

Actively search for these relationship types.

## Prerequisite

Concept A should normally be understood before Concept B.

```text
A
↓
B
```

## Depends On

Concept B functionally or logically depends on Concept A.

```text
B --depends on--> A
```

## Leads To

Understanding or introducing Concept A naturally leads to Concept B.

```text
A
↓
B
```

## Cause → Effect

```text
Cause
↓
Effect
```

## Problem → Solution

```text
Problem
↓
Technology / Concept
```

## Contrast

Concepts may appear similar but must be distinguished.

```text
[[Concept A]]
vs
[[Concept B]]
```

## Related

Use `related` when a stronger semantic relationship cannot be justified.

Do not label every connection as dependency.

---

# 10. Relationship Confidence

Do not convert uncertain inference into fact.

Relationships should conceptually fall into:

### Explicit

Directly supported by the source material.

### Strongly Inferred

Technically reasonable and strongly implied.

### REVIEW

Potentially useful but requires human confirmation.

When uncertain:

```markdown
## REVIEW

- Possible relationship:
  [[Concept A]] → [[Concept B]]

  Reason:
  ...

  Needs manual verification.
```

Do not fabricate certainty.

---

# 11. Use Obsidian Wikilinks

Use Obsidian wikilinks for established Concepts and Notes.

Example:

```markdown
[[VLAN]]
[[STP]]
[[Routing Table]]
```

Prefer links to existing notes.

Do not create links merely to increase Graph View connectivity.

Every link should have semantic value.

---

# 12. Update Existing Concepts

When a processed Unit provides additional useful information about an existing Concept, update the existing Concept Note when appropriate.

Possible additions:

- new source reference
- new prerequisite
- new relationship
- new contrast
- improved explanation
- important example
- new question

Do not overwrite good existing explanations unnecessarily.

Preserve useful manually written material.

Prefer additive changes.

---

# 13. Generate or Update Knowledge Maps

After concept extraction, inspect:

```text
04-Maps/
```

Determine whether the Unit contributes to an existing Knowledge Map.

Prefer updating an existing map over creating a duplicate map.

Create a new Map only when the material forms a meaningful reusable knowledge structure.

Useful Map forms include:

### Dependency Chain

```text
[[A]]
 ↓
[[B]]
 ↓
[[C]]
```

### Problem → Solution

```text
Problem
 ↓
[[Solution]]
```

### Technology Evolution

```text
Requirement
 ↓
Technology A
 ↓
Limitation
 ↓
Technology B
```

### Hierarchy

```text
Topic
├── [[Concept A]]
├── [[Concept B]]
└── [[Concept C]]
```

### Cause and Effect

```text
Cause
 ↓
Effect
 ↓
Consequence
```

### Troubleshooting Flow

```text
Symptom
 ↓
Check A
 ↓
Check B
 ↓
Root Cause
```

Maps should emerge from concepts.

Do not create Maps merely because a Unit was processed.

Typical signals for creating a Map:

- three or more related Concepts
- a clear dependency chain
- a meaningful cause-effect sequence
- a technology hierarchy
- a reusable workflow
- a troubleshooting structure
- a cross-source concept cluster

---

# 14. Map Rules

A Knowledge Map is not a duplicate summary.

Maps should help answer:

- What depends on what?
- What comes before what?
- What causes what?
- Which concepts solve which problems?
- Which concepts belong together?
- Which concepts are easily confused?
- How does one subject lead into another?

Avoid maps that simply reproduce the source table of contents.

---

# 15. Generate Questions

For every processed Unit, create or update relevant questions under:

```text
05-Questions/
```

Question generation should emphasize reasoning.

Preferred question types:

## Why

- Why does this technology exist?
- Why is this mechanism necessary?
- Why does this behavior occur?

## What If

- What happens if this mechanism is removed?
- What happens if this prerequisite is missing?
- What happens when this condition changes?

## Relationship

- How are A and B related?
- Why does B depend on A?
- Why does A lead to B?

## Contrast

- What is the difference between A and B?
- When should A be used instead of B?
- Why are A and B often confused?

## Cause and Effect

- What causes this behavior?
- What consequences follow from this condition?

## Troubleshooting

- If this feature fails, what prerequisite should be checked first?
- Which symptom suggests failure at this layer?
- How would you distinguish failure A from failure B?

## Cross-Document / Cross-Unit

- Which earlier concept is required to understand this?
- Which later topics depend on this concept?
- Where else does this concept appear?
- How does one source extend or contradict another?

Avoid excessive definition-only questions such as:

```text
What is VLAN?
What is STP?
What is OSPF?
```

Prefer reasoning questions.

---

# 16. Cross-Document and Cross-Unit Analysis

When processing a Unit, inspect relevant existing:

```text
01-Units/
02-Source-Notes/
03-Concepts/
04-Maps/
```

Search for relationships including:

- prerequisite
- dependency
- continuation
- repeated concepts
- contrast
- cause-effect
- shared mechanisms
- shared protocols
- problem-solution relationships

For multi-document Units, explicitly look for relationships that emerge only when several documents are considered together.

Example:

```text
Document 1
[[Ethernet]]
     ↓
Document 2
[[Switching]]
     ↓
Document 3
[[VLAN]]
```

The resulting Unit-level chain may be:

```text
[[Ethernet]]
     ↓
[[Switching]]
     ↓
[[VLAN]]
```

even if the complete chain never appears in one document.

Do not create artificial relationships solely because two Concepts appear in the same source.

---

# 17. Cross-Source Relationships

When multiple books or sources exist in the Vault, concepts should not be isolated by source.

Example:

```text
Book A
   ↓
[[VLAN]]
   ↑
Book B
```

The Concept Note belongs to the knowledge base, not to a specific book.

Different sources may:

- reinforce a concept
- explain it differently
- provide different examples
- disagree
- provide additional detail

Record meaningful differences when useful.

Do not automatically duplicate Concept Notes for each source.

---

# 18. Preserve Source Traceability

Important concepts should remain traceable to their source.

Where useful, record:

```markdown
## Appears In

- [[Source-Note-A]]
- [[Source-Note-B]]
```

Maintain a distinction between:

```text
Source statement
vs
AI inference
vs
User understanding
```

Do not claim that an inference comes directly from a source when it was generated through reasoning.

---

# 19. Knowledge Consistency

Maintain consistent terminology throughout the Vault.

Before creating a name:

1. Search existing Concept Notes.
2. Prefer established terminology.
3. Use the same capitalization and naming style.
4. Avoid synonymous duplicate files.

If multiple names are common, use one canonical Concept Note and mention aliases inside it.

---

# 20. Do Not Over-Link

Links should represent meaningful knowledge relationships.

Bad linking:

```text
Every noun → [[link]]
```

Good linking:

```text
[[VLAN]]
 ↓
[[802.1Q]]
 ↓
[[Trunk]]
```

Graph density is not a goal.

Knowledge clarity is the goal.

---

# 21. Questions as Knowledge Gaps

Questions are not only quiz material.

They may also represent uncertainty.

Examples:

```markdown
- Why does this implementation behave differently?
- Is this relationship always true or only under certain conditions?
- Does Source B contradict Source A?
```

If unresolved, mark them:

```text
REVIEW
```

Do not force an answer when the sources are insufficient.

---

# 22. REVIEW Policy

Use `REVIEW` whenever:

- source evidence is weak
- terminology is ambiguous
- multiple possible relationships exist
- a technical inference requires validation
- two sources appear inconsistent
- a Concept might duplicate another Concept
- a Map relationship is uncertain

Example:

```markdown
## REVIEW

- Determine whether [[A]] truly depends on [[B]]
  or is only commonly associated with it.
```

Human review is preferred over false certainty.

---

# 23. Editing Policy

When modifying generated knowledge files, prefer:

```text
preserve
+
extend
+
refine
```

rather than:

```text
delete
+
rewrite everything
```

Do not delete manually written material unless explicitly instructed.

Do not replace user explanations merely because another wording is possible.

---

# 24. Batch Processing

When multiple Units are processed together:

Do not treat each Unit as an isolated task.

After individual extraction:

1. Identify repeated Concepts.
2. Merge duplicate concept candidates.
3. Identify cross-unit relationships.
4. Update Maps.
5. Identify prerequisite chains.
6. Identify major knowledge hubs.
7. Generate cross-unit Questions.
8. Flag inconsistencies.

Example:

```text
Unit A
  ↓
Concept 1
  ↓
Concept 2

Unit B
  ↓
Concept 2
  ↓
Concept 3

Combined knowledge:

Concept 1
   ↓
Concept 2
   ↓
Concept 3
```

---

# 25. Periodic Knowledge Review

After several Units have been processed, perform a higher-level review.

Recommended interval:

```text
3–5 Units
```

or whenever a major topic group has been completed.

During review, identify:

### Knowledge Chains

```text
A → B → C → D
```

### Knowledge Hubs

Concepts referenced by many other Concepts.

### Orphan Concepts

Concept Notes with very few meaningful relationships.

### Duplicate Concepts

Different files representing essentially the same idea.

### Missing Prerequisites

A concept requiring knowledge that is not yet represented.

### Missing Maps

Concept clusters that would benefit from a Map.

### Knowledge Gaps

Important unanswered questions.

---

# 26. Orphan Concept Review

An isolated Concept is not automatically wrong.

However, inspect Concepts that have:

- no prerequisite
- no related concepts
- no incoming references
- no outgoing relationships

Ask whether:

1. the Concept should connect to existing knowledge;
2. the Concept is too minor to deserve a standalone file;
3. prerequisite knowledge is missing;
4. the Concept is genuinely independent.

Do not create arbitrary links just to eliminate orphan nodes.

---

# 27. Knowledge Hub Review

Concepts referenced across many Units may represent core knowledge.

When a Knowledge Hub emerges, improve that Concept Note.

Consider adding:

- clearer definition
- Why It Exists
- prerequisite chain
- contrasts
- examples
- important relationships
- relevant Questions
- dedicated Knowledge Map

Core Concepts deserve more refinement than peripheral Concepts.

---

# 28. Source Notes vs Concept Notes

Maintain the distinction:

## Source Note

Answers:

> What does this Unit or source group teach?

## Concept Note

Answers:

> What do I know about this idea across all sources?

Example:

```text
Source Note:
Unit-A

       ↓ refers to

Concept:
[[VLAN]]

       ↑ also referred to by

Source Note:
Unit-B
```

Do not turn Source Notes into duplicate Concept Notes.

---

# 29. Maps vs Source Structure

The source structure represents how the author chose to explain material.

Knowledge Maps represent how concepts relate.

These are different.

Example source structure:

```text
Document 1
Document 2
Document 3
Document 4
```

Knowledge Map may instead be:

```text
[[Ethernet]]
     ↓
[[Switching]]
     ↓
[[VLAN]]
     ↓
[[Trunk]]
     ↓
[[STP]]
```

Do not force knowledge organization to follow source order.

---

# 30. Focus on "Why"

Whenever possible, capture:

```text
Why does this exist?
```

before merely recording:

```text
What is this?
```

For technical subjects, useful knowledge often follows:

```text
Problem
   ↓
Requirement
   ↓
Concept / Technology
   ↓
Mechanism
   ↓
Limitation
   ↓
Next Concept
```

This sequence is especially valuable for Knowledge Maps.

---

# 31. Focus on Mechanisms

For important Concepts, identify:

```text
Input
 ↓
Mechanism
 ↓
Output
```

or:

```text
Condition
 ↓
Process
 ↓
Result
```

Mechanism knowledge is often more reusable than definition memorization.

---

# 32. Commands and Examples

Commands, configuration examples, formulas, and code may remain in Source Notes unless they have broader reusable value.

Do not automatically create a Concept Note for every command.

Where appropriate, connect examples to concepts.

Example:

```markdown
## Important Commands

`show vlan brief`

Related concept:

[[VLAN]]
```

---

# 33. Source Evidence vs General Knowledge

Primary analysis should be grounded in the provided source material.

General technical knowledge may be used to identify possible relationships, but:

- do not silently mix outside knowledge with source claims;
- mark uncertain additions as REVIEW;
- clearly distinguish source-supported statements from inferred relationships when material.

---

# 34. Final Processing Report

After processing a Unit or group of Units, provide a concise execution report.

Use:

```markdown
## Created

- ...

## Updated

- ...

## Reused

- ...

## Relationships Added

- ...

## REVIEW

- ...
```

### Created

List newly created:

- Source Notes
- Concept Notes
- Maps
- Question files

### Updated

List existing files that were modified.

### Reused

List existing Concepts reused instead of creating duplicates.

### Relationships Added

List important relationships discovered.

Each relationship listed here must also be recorded durably in `04-Maps/`.

When reporting, include the Map file where the relationship was recorded.

### REVIEW

List items requiring human validation.

Each REVIEW item listed here must also be recorded durably in `06_review/`.

When reporting, include the Review file where the item was recorded.

Do not merely respond:

```text
Done.
```

---

# 35. Core Principle

The goal is NOT:

```text
Source
↓
shorter summary
```

The goal is:

```text
Source
      ↓
Units
      ↓
Concepts
      ↓
Relationships
      ↓
Knowledge Network
      ↓
Understanding
```

Codex should reduce mechanical note-taking work so that the user can spend more time validating, understanding, questioning, and connecting important ideas.

---

# 36. Quality Standard

A good processing result should make it easier to answer:

1. What is this concept?
2. Why does it exist?
3. What problem does it solve?
4. What must I understand before learning it?
5. What depends on it?
6. What does it lead to?
7. What concepts are easily confused with it?
8. Where else does it appear?
9. How does it connect to the broader knowledge structure?
10. What am I still uncertain about?

If the generated notes cannot help answer these questions, improve the knowledge structure rather than generating more text.
