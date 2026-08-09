---
name: project-brief
description: >
  This skill should be used when the user asks to "write a project brief",
  "create a project brief for [project]", "draft a BA brief", "put together
  a project brief", or pastes a plain-language project description and wants
  a formal Project Brief produced from it. Generates a structured Word
  (.docx) Project Brief covering business problem, objectives, scope,
  success criteria, stakeholders, timeline, assumptions, constraints, risks,
  and an approval block.
metadata:
  version: "0.1.0"
---

# Project Brief Generator

Produce a professional, enterprise-ready Project Brief from a plain-language
project description. The person using this skill is a Business Analyst —
assume BA domain fluency but no technical or AI-prompting knowledge. They
should be able to open the resulting document and only need to fill in a
handful of specifics before circulating it.

## Step 1: Read the input

Accept whatever project description the user provides — this can be a
sentence, a paragraph, or several paragraphs of unstructured plain language.
Do not ask the user to reformat it or answer a questionnaire first.

If the input follows this plugin's standard prompt template (see
`PROMPT_TEMPLATES.md` at the plugin root — labeled fields such as
`Project Name:`, `Problem / Opportunity:`, `Objectives:`, `In Scope:`,
`Out of Scope:`, `Key Stakeholders:`, `Known Timeline / Deadline:`,
`Known Constraints:`), map each labeled field directly into its
corresponding section below instead of re-extracting it from prose. The
template is optional and exists to make output more consistent across a
team — free-form text works exactly the same way through inference.

Also check whether an organization template is available for this
document type: either a file the user has uploaded or referenced in this
conversation, or a bundled example at
`${CLAUDE_PLUGIN_ROOT}/org-templates/project-brief-example.docx` (see
`org-templates/README.md` for how organizations add these). If one
exists, mirror its section structure, headings, and tone as closely as
possible while still covering every section required below. If none is
available, use this skill's default structure.

Identify the project name from the description. If no name is stated or
implied, ask the user for one before proceeding — everything else in this
skill can be inferred, but a nameless document looks unfinished.

If the description is extremely thin (roughly a single sentence with no
indication of the problem being solved, who's affected, or what the
project involves), ask exactly one clarifying question targeting the
biggest gap, then proceed. Do not ask more than one question — the
point of this skill is a fast, low-friction draft, not an interview.

## Step 2: Draft each section

Write in formal, third-person, organizational voice — the tone of a
document that will circulate to a sponsor and steering committee, not a
casual summary. Avoid "I" statements. Use precise BA terminology
(objectives, scope, success criteria, assumptions, constraints) rather than
generic project-management language.

Ground every sentence in what the input actually says. Where a section
needs content the input doesn't provide, write a reasonable, specific
professional draft (not a generic placeholder) and mark it with an italic
tag at the end of the sentence or bullet: `*(assumption — confirm)*`. Never
fabricate hard numbers, dates, or budget figures — if a timeline or budget
isn't stated, describe it in relative/phase terms instead of inventing a
figure.

Generate these sections, in order:

1. **Header block** — Project Name, Prepared By (leave as `[Name]` for the
   BA to fill in unless the user has told you their name earlier in the
   conversation), Date (today's date), Status: Draft v0.1.

2. **Executive Summary** — 3–5 sentences: the problem, the proposed
   response, and the expected value. This is the only section a busy
   sponsor may read in full, so it must stand alone.

3. **Business Problem / Opportunity** — What is driving the need for this
   project. Root cause or opportunity, not just symptoms.

4. **Project Objectives** — 3–6 bullets, each a specific and measurable
   outcome (SMART-style) tied directly back to the problem statement.

5. **Scope** — Two labeled lists: **In Scope** and **Out of Scope**. Always
   populate Out of Scope even if the input doesn't mention it directly —
   infer reasonable exclusions from what's in scope and mark them as
   assumptions. A brief with no stated exclusions is a common real-world BA
   mistake; don't reproduce it.

6. **Success Criteria** — Measurable criteria or KPIs that define "done."
   Tie each one back to an objective from Section 4.

7. **Stakeholders (high-level)** — A short table: Name/Role, Department or
   Organization, Interest in the Project (one phrase). Keep this brief —
   note under the table that a detailed Stakeholder Analysis with a
   Power/Interest Matrix is available as a separate document via this
   plugin's Stakeholder Analysis skill.

8. **High-Level Timeline / Milestones** — A table: Milestone, Target
   Phase or Date. If no dates are given, use relative phases (Discovery,
   Design, Build, Testing, Rollout) rather than inventing calendar dates.

9. **Assumptions & Constraints** — Bulleted list. Include both assumptions
   made while drafting this brief and any constraints stated or implied in
   the input (budget ceiling, regulatory requirement, fixed deadline,
   legacy system dependency, etc.).

10. **High-Level Risks** — A table: Risk, Potential Impact, Mitigation
    (one line each). 3–5 risks grounded in the specific project context —
    not a generic boilerplate risk list.

11. **Approval** — A signature block with labeled blank lines for Prepared
    By, Business Sponsor, and Date, formatted for sign-off.

## Step 3: Build the document

Invoke this environment's docx skill/capability to produce the file as a
formatted Word document — do not just output markdown in chat. Use Heading
1 for the title, Heading 2 for each numbered section, and real Word tables
(not markdown tables) for the Stakeholders, Timeline, and Risks sections.
Keep formatting clean and consistent with standard enterprise document
conventions: readable body font, consistent heading styles, no decorative
elements.

## Step 4: Name, save, and summarize

Name the file `{ProjectName}_ProjectBrief.docx`, using the project name in
Title Case with spaces replaced by underscores and special characters
stripped (e.g. `Vendor Data Onboarding` → `Vendor_Data_Onboarding_ProjectBrief.docx`).

After the file is created, tell the user in 1–2 sentences that the brief is
ready and how many `(assumption — confirm)` tags are in it, so they know
exactly how much review is needed before circulating it. Do not restate the
document's contents in chat — the user can open the file.

## Example

Input: *"We need to bring on a new lab results vendor and get their data
flowing into our system without breaking existing reports."*

Generated Business Problem sentence: "The organization has selected a new
laboratory results vendor whose data must be integrated into existing
clinical systems without disrupting current reporting workflows, which are
depended on by downstream clinical and administrative teams *(assumption —
confirm)*."

Note how the stated fact (new vendor, integration, don't break reports) is
kept as-is, and the added detail (who depends on the reports) is inferred
and flagged rather than stated as fact.
