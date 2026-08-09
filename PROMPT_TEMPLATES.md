# Prompt Templates

These templates are optional. Every skill in this plugin works fine with a
plain paragraph of project description — it will infer what it needs and
flag anything it assumed. Use these templates when you want more
consistent, standardized output across a team, or when you already know
specifics (stakeholder names, known constraints, etc.) and want to make
sure they're used exactly as given rather than inferred.

To use one, copy the template, fill in what you know, leave the rest
blank, and paste it as your message to the skill. Blank fields are simply
inferred, same as if you'd written free text.

---

## Project Brief

```
Write a project brief for the following:

Project Name:
Problem / Opportunity:
Objectives (what should this achieve):
In Scope:
Out of Scope (if known):
Key Stakeholders (name/role — why they matter):
Known Timeline / Deadline (if any):
Known Constraints (budget, regulatory, technical, etc.):
```

## Stakeholder Analysis

```
Create a stakeholder analysis for the following project:

Project Name:
Project Description:
Known Stakeholders (name/role — importance/why they matter, one per line):
-
-
-

(Leave the stakeholder list blank if unsure — the skill will infer likely
stakeholders from the project description.)
```

## RACI Chart

```
Create a RACI chart for the following project:

Project Name:
Project Description:
Key Activities/Phases (leave blank to infer):
-
Roles Involved (name/title — leave blank to infer):
-
```

## Process Flow

```
Map the process flow for the following:

Project Name:
Process Description:
Scope: [ ] Current State   [ ] Future State   [ ] Both
Actors/Systems Involved (if known):
```

## Requirements Document

```
Write a requirements document for the following:

Project Name:
Project Description:
Business Needs/Goals (if known):
Known Functional Needs (specific system/process behaviors, if any):
Known Non-Functional Needs (performance, security, compliance, etc.):
Anything explicitly out of scope for this phase (if any):
```

---

## Organization templates

If your organization has its own standard format for any of these
documents, you don't have to choose between this plugin's default
structure and your org's format. See `org-templates/README.md` for how to
either upload your org's template in the conversation each time, or bundle
it into this plugin once so every BA using it automatically gets
org-matched output.
