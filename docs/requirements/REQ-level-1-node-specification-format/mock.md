# Mock: Level 1 Node Specification Format

## File layout

```
docs/process/nodes/
├── identify-course.md
├── define-campaign.md
├── reach-audience.md
├── capture-leads.md
└── ...
```

## Example Level 1 node file

```
docs/process/nodes/capture-leads.md
+------------------------------------------------------------+
| ---                                                          |
| id: capture-leads                                            |
| name: Capture Leads                                          |
| input: Campaign response (landing page submission)           |
| output: Identifiable lead record                             |
| next_nodes:                                                  |
|   - nurture-leads                                            |
| notes: null                                                  |
| ---                                                           |
|                                                                |
| Capture people who respond to the campaign and create        |
| identifiable lead records.                                   |
+------------------------------------------------------------+
```

## Validation outcome (illustrative)

```
+------------------------------------------------------------+
| Validating docs/process/nodes/capture-leads.md               |
+------------------------------------------------------------+
| [ok]   id present:            capture-leads                 |
| [ok]   name present:          Capture Leads                  |
| [ok]   description body:      non-empty                      |
| [ok]   next_nodes resolve:    nurture-leads.md exists         |
| [ok]   no implementation/tools/automation keys                |
+------------------------------------------------------------+
| VALID                                                          |
+------------------------------------------------------------+
```
