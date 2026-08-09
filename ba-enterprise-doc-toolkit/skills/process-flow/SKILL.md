---
name: process-flow
description: >
  This skill should be used when the user asks to "map a process flow",
  "document the current state process", "future state process", "swimlane
  diagram", "as-is / to-be process", or provides a project description and
  wants a process broken into sequential steps by actor. Produces a Word
  (.docx) document with structured step tables (not a diagram) formatted
  so the steps can be pasted directly into Visio or Lucidchart.
metadata:
  version: "0.1.0"
---

# Process Flow Generator

Produce a structured, step-by-step breakdown of a business process as
tables — one row per step, labeled by actor/swimlane — that a BA can lift
directly into a swimlane diagramming tool. This skill does not generate an
actual diagram; it produces the structured content a diagram is built
from. Say this plainly to the user if they ask for a "diagram" or
"flowchart": this environment doesn't render flowcharts, but the output is
built to paste straight into Visio or Lucidchart's swimlane templates.

## Step 1: Determine scope — Current State, Future State, or both

Infer from the input: if it describes an existing process being replaced,
fixed, or improved, generate **both** Current State and Future State. If
it only describes a new process being designed from scratch, generate
**Future State only**. If it's genuinely ambiguous which is wanted, ask the
user once: "Do you want the current process, the proposed future process,
or both?" — then proceed without further questions.

## Step 2: Identify actors and build the step sequence

Identify the swimlanes: the roles, teams, or systems that perform steps in
the process (e.g. End User, Business Analyst, Vendor System, Approval
Queue). Every step must belong to exactly one lane.

Build the sequence starting from a clear trigger/start event and ending at
a clear end event — do not leave the process open-ended. For each step,
capture:

- **Step #** — sequential
- **Swimlane/Actor** — who performs this step
- **Action** — what happens, described as a single clear action
- **Decision point?** (Y/N) — if yes, phrase the step as a question
  (e.g. "Does the data pass validation?")
- **Next step if Yes / Next step if No** — for decision points, the step
  number each branch continues to (or "End")
- **System/Tool used** — if the input names or implies a system
- **Notes** — anything worth flagging, including `(assumption — confirm)`
  for inferred steps

For **Current State**, add a **Pain Point / Issue** column and populate it
wherever the process implies inefficiency, manual rework, delay, or risk —
this is often the most valuable part of an as-is map for a BA.

For **Future State**, add a short **Key Differences from Current State**
section after the table, summarizing what changed and why, tied back to
the pain points identified in the Current State map (skip this section if
Future State is being generated alone).

## Step 3: Write a narrative walkthrough

Before each table, write a 3–5 sentence plain-language walkthrough of the
process end to end, so a reader who won't trace the full table still
understands the flow.

## Step 4: Build the document

Invoke this environment's docx skill/capability. Structure:

1. Brief note at the top: "This document maps the process in structured
   steps for use in Visio or Lucidchart. Each row corresponds to one shape
   in a swimlane diagram — decision points map to decision diamonds, and
   the Swimlane/Actor column indicates which lane each step belongs in."
2. Current State section (if in scope): narrative + step table.
3. Future State section (if in scope): narrative + step table + Key
   Differences summary.

Use real Word tables, Heading 1 for the document title, Heading 2 for each
state's section header.

## Step 5: Name, save, and summarize

Name the file `{ProjectName}_ProcessFlow.docx` (or
`{ProjectName}_ProcessFlow_CurrentState.docx` if only current state was
requested, `{ProjectName}_ProcessFlow_FutureState.docx` if only future
state).

After the file is created, tell the user in 1–2 sentences which state(s)
were mapped, how many steps and decision points are in each, and remind
them the table is meant to be pasted into their diagramming tool of
choice.
