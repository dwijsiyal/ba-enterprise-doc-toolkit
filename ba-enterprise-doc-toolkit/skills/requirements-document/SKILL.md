---
name: requirements-document
description: >
  This skill should be used when the user asks to "write a requirements
  document", "create functional requirements", "business requirements",
  "non-functional requirements", "MoSCoW prioritization", or provides a
  project description and wants formal requirements drafted with
  traceability and priority. Produces a Word (.docx) Requirements Document
  with Business, Functional, and Non-Functional requirements, each with a
  unique ID, MoSCoW priority, and traceability back to a business need.
metadata:
  version: "0.1.0"
---

# Requirements Document Generator

Produce a formal Requirements Document from a plain-language project
description, structured in the three standard BA layers: Business,
Functional, and Non-Functional, each MoSCoW-prioritized and traceable.

If the input follows this plugin's standard prompt template (see
`PROMPT_TEMPLATES.md` at the plugin root — labeled fields such as
`Project Name:`, `Project Description:`, `Business Needs/Goals:`,
`Known Functional Needs:`, `Known Non-Functional Needs:`, `Anything
explicitly out of scope:`), map each labeled field directly instead of
re-extracting it from prose. The template is optional; free-form text
works the same way through inference.

Also check whether an organization template is available for this
document type: either a file the user has uploaded or referenced in this
conversation, or a bundled example at
`${CLAUDE_PLUGIN_ROOT}/org-templates/requirements-document-example.docx`
(see `org-templates/README.md` for how organizations add these). If one
exists, mirror its section structure and requirement ID conventions as
closely as possible while still covering everything required below. If
none is available, use this skill's default structure.

## Step 1: Draft Business Requirements (BR)

Business Requirements state *what the organization needs*, independent of
any system or solution. Derive 3–8 from the project description — these
should read like organizational goals ("The organization must be able to
onboard a new data vendor without disrupting existing reporting"), not
system behaviors. Assign each a sequential ID: `BR-01`, `BR-02`, etc.

## Step 2: Draft Functional Requirements (FR)

Functional Requirements state *what the system or process must do*,
specific enough to design and test against. Derive these from the project
description and expand each Business Requirement into one or more
Functional Requirements that would satisfy it. Assign sequential IDs:
`FR-01`, `FR-02`, etc. For each, include a one-sentence **Acceptance
Criteria** ("The system shall reject any record missing a required field
and log the rejection reason") so the requirement is testable, not just
descriptive.

Every Business Requirement should be traceable to at least one Functional
Requirement. If the input doesn't give enough detail to fully satisfy a
Business Requirement with concrete Functional Requirements, write the best
professional draft you can and mark the gap with `(assumption — confirm)`
rather than leaving it thin.

## Step 3: Draft Non-Functional Requirements (NFR)

Non-Functional Requirements state *how well* the system must perform,
grouped by category: Performance, Security, Availability, Usability,
Maintainability, and Compliance/Privacy. Derive these from what the input
states or implies. For healthcare and government projects specifically,
proactively consider and include NFRs for data privacy and regulatory
compliance (e.g. handling of personal health information, audit logging,
data retention) even if the input doesn't explicitly mention them — mark
these as `(assumption — confirm)` since they're inferred from domain
context rather than stated directly. Assign sequential IDs: `NFR-01`,
`NFR-02`, etc.

## Step 4: Assign MoSCoW priority and rationale

For every requirement (BR, FR, and NFR), assign one of:

- **Must** — critical; the project fails without it
- **Should** — important, but the project can launch without it if needed
- **Could** — desirable, lower-impact if descoped
- **Won't** — explicitly out of scope for this phase (include only if the
  input implies something was deliberately descoped — don't invent Won't
  items)

Include a one-sentence rationale for each priority, and for every FR and
NFR, a **Source** column referencing the BR ID(s) it traces back to.

## Step 5: Build the document

Invoke this environment's docx skill/capability. Structure:

1. Short intro paragraph explaining the document's purpose and that
   requirements are traceable from Business → Functional/Non-Functional.
2. **Business Requirements** — table: ID, Requirement, Priority, Rationale.
3. **Functional Requirements** — table: ID, Requirement, Acceptance
   Criteria, Priority, Source (BR ID), Rationale.
4. **Non-Functional Requirements** — table grouped by category: ID,
   Requirement, Priority, Source, Rationale.
5. A short **Traceability Summary** note at the end stating how many FRs
   and NFRs trace to each BR, and flagging any BR with no traced FR so the
   BA can address the gap.

Use Heading 1 for the title, Heading 2 for each requirement layer, real
Word tables throughout.

## Step 6: Name, save, and summarize

Name the file `{ProjectName}_RequirementsDocument.docx` (Title Case with
underscores, special characters stripped).

After the file is created, tell the user in 1–2 sentences the total
requirement count by layer (BR/FR/NFR) and flag if any Business
Requirement has no traced Functional Requirement, so they know exactly
where to focus review.
