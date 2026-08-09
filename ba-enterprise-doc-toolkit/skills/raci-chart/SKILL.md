---
name: raci-chart
description: >
  This skill should be used when the user asks to "create a RACI chart",
  "RACI matrix for [project]", "who's responsible for", "responsibility
  assignment matrix", or provides a project description and wants
  activities mapped to roles as Responsible/Accountable/Consulted/Informed.
  Produces an Excel (.xlsx) RACI matrix with validated accountability
  assignments and a legend tab.
metadata:
  version: "0.1.0"
---

# RACI Chart Generator

Produce a RACI matrix (Responsible, Accountable, Consulted, Informed) from
a plain-language project description.

## Step 1: Identify activities (rows)

If the user supplies a list of activities or phases, use it. Otherwise,
infer activities from the project description, grouped under standard BA
project phases: Discovery/Requirements, Design, Build/Configuration,
Testing/UAT, Training, Go-Live/Deployment, Post-Go-Live Support. Break each
phase into 2–4 specific activities drawn from what the input describes
(e.g. for a data integration project: "Define data mapping specification,"
"Configure vendor data feed," "Execute UAT test cases," "Cut over to
production feed") rather than generic phase names alone. Aim for 10–20
activity rows total — enough to be a real working RACI, not a token
gesture.

## Step 2: Identify roles (columns)

Infer roles from the project description: typically a mix of Project
Sponsor, Business Analyst, Project Manager, Technical/IT Lead, Vendor
(if external systems or vendors are involved), End Users or clinical/
operational staff, and Privacy/Compliance Officer or QA/Testing Lead where
the project context implies them. Keep the role list to 5–8 columns —
more than that makes the matrix unreadable in Excel.

## Step 3: Assign R/A/C/I

For every activity/role cell, assign one of: R (Responsible — does the
work), A (Accountable — owns the outcome and signs off), C (Consulted —
provides input before the work is done), I (Informed — notified after),
or leave blank if the role has no involvement in that activity.

Apply RACI best practice: **exactly one "A" per row.** Multiple people can
be Responsible, but accountability must be singular or the matrix is
unusable in practice. As you assign each row, check this rule. If the
project description genuinely implies shared or unclear accountability for
an activity, make a reasonable resolution (assign the "A" to whichever
role has ultimate sign-off authority for that activity type) and note the
resolution in that row's Notes column rather than leaving multiple A's.

## Step 4: Build the workbook

Invoke this environment's xlsx skill/capability to produce a real Excel
file with two tabs:

**Tab 1 — "RACI Matrix"**: activities as rows (grouped visually by phase,
e.g. with a bolded phase-header row above each group), roles as column
headers, R/A/C/I in the cells, plus a final "Notes" column for any
resolution notes from Step 3. Freeze the header row and the activity-name
column. Distinctly color-fill cells containing "A" so accountability is
visually scannable across the sheet.

**Tab 2 — "Legend"**: define R/A/C/I plainly, and list any rows where
accountability required a judgment call, with a one-line explanation for
each, so the BA can quickly review and adjust anything that doesn't match
how their organization actually works.

## Step 5: Name, save, and summarize

Name the file `{ProjectName}_RACIChart.xlsx` (Title Case with underscores,
special characters stripped).

After the file is created, tell the user in 1–2 sentences how many
activities and roles were mapped, and how many rows (if any) needed a
judgment call on accountability — point them to the Legend tab for those.
