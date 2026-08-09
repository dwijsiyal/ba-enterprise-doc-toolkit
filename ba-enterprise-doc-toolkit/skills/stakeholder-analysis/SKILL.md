---
name: stakeholder-analysis
description: >
  This skill should be used when the user asks to "create a stakeholder
  analysis", "stakeholder analysis for [project]", "power interest matrix",
  "who are the stakeholders", "identify project stakeholders", or provides
  a project description and wants stakeholders identified, rated, and
  mapped. Produces an Excel (.xlsx) Stakeholder Register with Power/Interest
  ratings, quadrant classification, engagement strategy, and an actual
  Power/Interest Matrix scatter chart.
metadata:
  version: "0.1.0"
---

# Stakeholder Analysis Generator

Produce a Stakeholder Register and Power/Interest Matrix from a
plain-language project description. Target 6–12 stakeholders — enough to be
genuinely useful for engagement planning, not padded with irrelevant roles.

If the input follows this plugin's standard prompt template (see
`PROMPT_TEMPLATES.md` at the plugin root — labeled fields such as
`Project Name:`, `Project Description:`, `Known Stakeholders:`), map each
labeled field directly instead of re-extracting it from prose. The
template is optional; free-form text works the same way through
inference.

Also check whether an organization template is available for this
document type: either a file the user has uploaded or referenced in this
conversation, or a bundled example at
`${CLAUDE_PLUGIN_ROOT}/org-templates/stakeholder-analysis-example.xlsx`
(see `org-templates/README.md` for how organizations add these). If one
exists, mirror its structure, column naming, and rating scale as closely
as possible while still covering everything required below. If none is
available, use this skill's default structure.

## Step 1: Identify the stakeholders

If the user supplies a stakeholder list, use it as the base set. Whether or
not a list is supplied, also infer any stakeholders the project clearly
implies but the input doesn't name — a sponsor, an end-user group, an
IT/technical team, a compliance or privacy officer if the project touches
regulated or personal data, an external vendor or partner if one is
mentioned, or an oversight body (e.g. a ministry, board, or regulator) if
the context implies one. This matters especially for healthcare and
government projects, where privacy and oversight stakeholders are easy to
omit but are usually essential to a real stakeholder analysis.

Any stakeholder not explicitly named or described in the input is an
inferred stakeholder — track this so it can be flagged in Step 3.

## Step 2: Rate and classify each stakeholder

For each stakeholder, determine:

- **Power** (High / Medium / Low): their ability to influence project
  decisions, funding, or direction.
- **Interest** (High / Medium / Low): how much the project's outcome
  affects them or how much they care about it.
- **Rationale**: one sentence grounding the rating in their actual role —
  never leave a rating unjustified.
- **Quadrant**: derived directly from Power and Interest —
  High/High = Manage Closely, High Power/Low Interest = Keep Satisfied,
  Low Power/High Interest = Keep Informed, Low/Low = Monitor.
- **Engagement strategy**: one to two sentences tailored to both the
  quadrant and the specific role — not generic boilerplate. A "Manage
  Closely" sponsor and a "Manage Closely" clinical lead should get
  different strategies even though they share a quadrant.
- **Communication frequency**: Weekly, Biweekly, Monthly, or As-needed,
  consistent with the quadrant (Manage Closely and Keep Satisfied warrant
  more frequent touchpoints than Monitor).

## Step 3: Build the workbook

Invoke this environment's xlsx skill/capability to produce a real Excel
file with two tabs:

**Tab 1 — "Stakeholder Register"**: columns Name/Role, Organization or
Department, Power, Interest, Quadrant, Rationale, Engagement Strategy,
Communication Frequency. Freeze the header row. Add a data validation
dropdown (High/Medium/Low) on the Power and Interest columns so the BA can
adjust ratings without breaking formatting. Lightly color-code the
Quadrant column by category (e.g. distinct fill color per quadrant) so the
sheet is scannable at a glance. Append `(assumption — confirm)` to the
Rationale cell for any inferred stakeholder from Step 1.

**Tab 2 — "Power-Interest Matrix"**: build an actual XY scatter chart, not
an image or manual grid. Map Low/Medium/High to 1/2/3 on both axes, plot
each stakeholder as a point labeled with their name, add gridlines at 1.5
and 2.5 on both axes so the four quadrants are visually separated, and
title the axes "Interest →" and "Power →". Title the chart "Power/Interest
Matrix — {ProjectName}".

## Step 4: Name, save, and summarize

Name the file `{ProjectName}_StakeholderAnalysis.xlsx` (Title Case with
underscores, special characters stripped).

After the file is created, tell the user in 1–2 sentences how many
stakeholders were identified and how many of those were inferred rather
than explicitly named, so they know where to focus their review.

## Calibration example

Role: "Privacy Officer" on a project integrating a new external vendor's
data feed. Power: Medium — can block go-live over a compliance gap but
doesn't control project funding or scope. Interest: High — directly
accountable for any privacy exposure the integration creates. Quadrant:
Keep Satisfied leaning Manage Closely. Engagement strategy: "Involve early
in data-mapping and vendor-agreement review; require formal sign-off
before go-live rather than routine status updates." This is more specific
than a generic "keep them informed" line — match this level of specificity
for every stakeholder.
