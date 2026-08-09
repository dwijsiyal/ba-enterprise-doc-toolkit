# Organization Templates

Every skill in this plugin defaults to a standard BA document structure.
If your organization already has its own template — its own section
names, formatting conventions, header/footer style, or document tone —
you can have every skill match it instead, in one of two ways.

## Option 1: Provide it in the conversation (no setup required)

Upload or paste your organization's example document (or a description of
its structure) in the same conversation before asking for a document.
Each skill checks for this automatically and, if found, mirrors the
example's section structure, headings, and tone as closely as possible —
while still including every piece of required content the skill normally
generates. This works immediately, but only for that conversation; you'd
upload it again next time.

## Option 2: Bundle it into the plugin (org-wide, one-time setup)

Whoever deploys this plugin inside your organization's Cowork environment
can drop an example document into this folder, using these exact
filenames so each skill can find its match automatically:

| File | Matches skill |
|---|---|
| `org-templates/project-brief-example.docx` | Project Brief |
| `org-templates/stakeholder-analysis-example.xlsx` | Stakeholder Analysis |
| `org-templates/raci-chart-example.xlsx` | RACI Chart |
| `org-templates/process-flow-example.docx` | Process Flow |
| `org-templates/requirements-document-example.docx` | Requirements Document |

Once bundled, every BA using the plugin gets output matched to your
organization's standard automatically — no per-conversation upload
needed. This folder ships empty by default; skills fall back to their
built-in default structure for any document type without a matching
example file here.

A bundled example should be a real, representative document — an
actual past Project Brief with identifying details removed, for
instance — not a blank template. Skills work from structure and tone,
so a filled-in example gives better results than an empty shell.
