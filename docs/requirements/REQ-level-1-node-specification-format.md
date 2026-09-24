# Level 1 Node Specification Format

## Summary

Defines the general **Level 1 node specification format**: a reusable
schema/template for documenting "what must happen" at a process node,
independent of any specific process. This requirement does not author
Level 1 content for the example marketing process in the blueprint
(Identify Course, Define Campaign, ...); it defines the format any process
node can be documented with, per the blueprint's core principle
(`docs/project/blueprint.cleaned.html`, sections 3-4, 10): **Level 1
defines WHAT.**

## Level 1 field set

Per the blueprint's node schema (section 3), a node supports `id`, `name`,
`description`, `input`, `output`, `implementation`, `tools`, `automation`,
`next_nodes`, and `notes`. Level 1 documentation populates only the fields
that describe *what* the node is and does, independent of tools, software,
people, APIs, or implementation:

- `id` — unique node identifier.
- `name` — short node name (e.g. "Capture Leads").
- `description` — what must happen at this node, in tool-independent terms.
- `input` — what the node requires before it can run.
- `output` — what the node produces once complete.
- `next_nodes` — ids of the node(s) that logically follow.
- `notes` — supplemental context about the node itself (not how it's
  implemented).

`implementation`, `tools`, and `automation` are explicitly **excluded**
from Level 1 — they belong to Level 2 and the automation definition
(blueprint sections 5-6), and must be populatable independently without
touching Level 1 content.

## Storage and format

Each node's Level 1 documentation is a single markdown file:

```
docs/process/nodes/{node-id}.md
```

Each file has YAML frontmatter for the structured fields, followed by the
node's description as the document body:

```markdown
---
id: capture-leads
name: Capture Leads
input: Campaign response (landing page submission)
output: Identifiable lead record
next_nodes:
  - nurture-leads
notes: null
---

Capture people who respond to the campaign and create identifiable lead
records.
```

- `id`, `name`, and the body `description` are required.
- `input`, `output`, `next_nodes`, and `notes` are optional and left
  empty/omitted when not relevant, per the blueprint's "only relevant
  fields need to be populated" note.

## Validation

A Level 1 node file is valid when:

- Frontmatter parses as YAML and includes non-empty `id` and `name`.
- The document body (after frontmatter) is non-empty — this is the
  `description`.
- Any `next_nodes` entries reference `id`s of other node files that exist
  under `docs/process/nodes/`.
- The frontmatter contains no `implementation`, `tools`, or `automation`
  keys — those belong to Level 2, not this format.

Authoring and maintaining these files is expected to follow the same
process as other requirement/process documentation in this repo — edited
directly as markdown and reviewed like any other doc change.

## Acceptance criteria

- A node's Level 1 documentation can be authored as a single markdown file
  under `docs/process/nodes/{node-id}.md`, following the frontmatter +
  body format above.
- The format supports `id`, `name`, `description`, `input`, `output`,
  `next_nodes`, and `notes` — and only those fields.
- A Level 1 file contains no implementation, tooling, or automation
  detail; that content can be added later (Level 2) without editing the
  Level 1 file.
- Validation rejects a Level 1 file missing `id`, `name`, or a description
  body, or one that includes `implementation`/`tools`/`automation` keys.

## Out of scope

- Authoring actual Level 1 content for the example marketing process
  nodes (Identify Course -> ... -> Follow Up) — this requirement defines
  the format only.
- Level 2 (how), automation, and tool/URL documentation formats — separate
  requirements.
