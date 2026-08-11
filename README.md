# BA Enterprise Doc Toolkit

A Claude Cowork plugin that generates standard business analysis documents
from a plain-language project description entirely inside your
organization's own Claude Cowork environment.

# How to Install

- Step 1: Download file [PLUGIN PACKAGE](https://github.com/dwijsiyal/ba-enterprise-doc-toolkit/blob/master/ba-enterprise-doc-toolkit.plugin)
<img width="1822" height="647" alt="image" src="https://github.com/user-attachments/assets/c0905778-f7c9-47f5-bf27-0c27008faa1b" />

- Step 2: Open Claude Desktop.
- Step 3: Go to Customize -> Plugin -> Add -> Upload Plugin -> Drag and Drop the downloaded file.
<img width="507" height="347" alt="image" src="https://github.com/user-attachments/assets/3eb967f8-ac95-4234-9d4f-7189d6135a1e" />
<img width="1401" height="880" alt="image" src="https://github.com/user-attachments/assets/3126696b-6dbb-4d5f-afe6-d82ace3fc7b3" />
<img width="697" height="613" alt="image" src="https://github.com/user-attachments/assets/2deaf5f0-b633-4fb3-99db-d0a3b85d6490" />

## Why this exists

Most BA teams in regulated industries (healthcare, government, financial
services) are restricted by data governance policy from pasting internal
project details into external AI tools. This plugin is built to run inside
an organization's own Claude Cowork deployment, so nothing leaves the
environment your organization has already agreed to use Claude within. If
your organization has deployed Claude Cowork, this plugin adds BA
document generation without introducing any new data-handling risk.

## Components

Five skills, one per document type. Each is triggered independently — use
one or all of them depending on what you need.

| Skill | Trigger phrases (examples) | Output |
|---|---|---|
| `project-brief` | "write a project brief", "draft a BA brief" | `.docx` — Project Brief |
| `stakeholder-analysis` | "stakeholder analysis", "power interest matrix" | `.xlsx` — Stakeholder Register + Power/Interest Matrix chart |
| `raci-chart` | "RACI chart", "who's responsible for" | `.xlsx` — RACI Matrix + Legend |
| `process-flow` | "process flow", "current state process", "swimlane" | `.docx` — structured step tables (current and/or future state) |
| `requirements-document` | "requirements document", "MoSCoW", "functional requirements" | `.docx` — Business/Functional/Non-Functional requirements with traceability |

No MCP servers, agents, or hooks — every skill is a self-contained,
single-turn generation task with no external dependencies beyond Cowork's
built-in docx/xlsx capabilities.

## Setup

No configuration, API keys, or environment variables required. Install the
plugin in your Cowork environment and the skills are available immediately.

## Usage

Paste a plain-language description of your project — no technical
knowledge required — and ask for the document you need, e.g.:

> "Write a project brief for this: we're onboarding a new lab results
> vendor and need their data flowing into our reporting system without
> breaking existing reports."

Each skill drafts the document, flags anything it inferred rather than
was told (marked `(assumption — confirm)`), and produces a real,
editable Word or Excel file — not a markdown dump in chat. Review the
flagged assumptions, edit as needed, and the document is ready to
circulate.

You can run skills independently in any order — there's no requirement to
generate a Project Brief before a Requirements Document, for example,
though many BAs will find that a natural sequence.

## Design principles

- **One skill per document type.** Generate only what you need, not an
  all-or-nothing bundle.
- **Plain-language input.** The BA describes the project like they would
  to a colleague; the plugin handles the structure.
- **Editable output, not final output.** These skills produce a strong
  first draft. The BA makes the final judgment calls — inferred content is
  always flagged for review, never silently presented as fact.

## Prompt Templates

- **Each skill has a template the an individual can follow to get a near standardized result** see [PROMPT_TEMPLATES](https://github.com/dwijsiyal/ba-enterprise-doc-toolkit/blob/master/PROMPT_TEMPLATES.md)

## License

MIT — see [LICENSE](LICENSE).
