# Impl: Level 1 Node Specification Format

## Requirement

`REQ-level-1-node-specification-format` — defines the general Level 1
node markdown/YAML format (`docs/process/nodes/{node-id}.md`: YAML
frontmatter with `id`, `name`, `input`, `output`, `next_nodes`, `notes`,
plus a markdown body as `description`) and its validation rules. See the
requirement doc for the full field spec and validation rules.

## Application

`app-prj-test2-svc` (services app). This work is a documentation-format
validator, not a UI feature — no screen or user flow is implied by the
requirement, so it belongs with backend/tooling rather than `webapp`.

## Approach

1. Add a validator that scans `docs/process/nodes/*.md`, parses YAML
   frontmatter (delimited by `---`) plus the remaining markdown as the
   body/description, and checks:
   - `id` and `name` present and non-empty in frontmatter.
   - Body (after frontmatter) non-empty — this is the `description`.
   - `next_nodes` (if present) is a list of ids, each of which resolves to
     an existing `docs/process/nodes/{id}.md` file.
   - Frontmatter contains none of `implementation`, `tools`, `automation`
     — fail validation if any are present, since those are Level 2/
     automation fields excluded from this format.
   - No other frontmatter keys beyond `id`, `name`, `input`, `output`,
     `next_nodes`, `notes` (keeps the format from silently absorbing
     Level 2 content under a different key name).
2. Expose it as a script (e.g. `scripts/validate-level1-nodes.*`) runnable
   locally and in CI, producing a pass/fail per file plus a summary,
   matching the mock's validation-output shape (`mock.md`).
3. Non-zero exit code on any validation failure, so it can gate CI.

## Files/areas likely affected

- New validator script/module under the `svc` app (exact path/language
  depends on `svc`'s existing stack — not yet inspected as part of this
  work item).
- Possibly a CI workflow step to run the validator against
  `docs/process/nodes/**/*.md` on PRs touching that path.

## Assumptions / open risks

- `app-prj-test2-svc` is newly created; its existing language/tooling
  stack (Node, Python, etc.) wasn't inspected before drafting this plan.
  The validator's implementation language should match whatever `svc`
  already uses once that's confirmed.
- No `docs/process/nodes/` directory exists yet in the `pro-test` repo —
  this work item covers the validator itself; it does not include
  authoring any actual node files (that's explicitly out of scope per the
  requirement).
