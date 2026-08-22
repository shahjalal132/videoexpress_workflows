# Common workflow blocks — run instruction + standing authorization

Reusable, workflow-agnostic prompt blocks that fix the two behaviours which stall autonomous runs:

1. the agent reads a pasted prompt as a **document to review** instead of instructions to execute, and
2. the agent asks the user for **permission** to do work the user already authorized by starting the run.

Nothing in either block names a specific app, tool, or step, so the same text works for the VOX documentary workflow, the nursery-rhyme workflow, and anything added later.

## Files

| File | Use it when |
|---|---|
| `RUN_INSTRUCTION_BLOCK.md` | Writing a **system prompt** — paste FIRST, above everything else |
| `PERMISSIONS_BLOCK.md` | Writing a **system prompt** — paste SECOND, after the role sentence |
| `permissions_rules.json` | Writing a **machine-readable contract** — merge these keys into the workflow's JSON (covers both blocks) |

The Markdown and the JSON express the same rules; keep them in sync if you edit one.

## Paste order in a system prompt

```
▶ RUN INSTRUCTION BLOCK      <- very first: "this document IS your instructions, start now"
   role sentence              <- "You are an autonomous agent that ..."
⛔ STANDING AUTHORIZATION     <- "nothing left to approve, never ask permission"
   ... the workflow itself ...
⛔ FINAL REMINDER             <- optional last line, from the bottom of PERMISSIONS_BLOCK.md
```

First and last tokens carry the most weight; the rules in between are the easiest for a model to lose, which is why the authorization is restated at the end.

## How to fill the slots

**In `RUN_INSTRUCTION_BLOCK.md`:**

- `{FIRST_ACTION}` — literally what the first reply must be (e.g. *"verify both tools are signed in, then send the intake message asking for script source, ratio and duration"*).
- `{WORKFLOW_NAME_AND_GOAL}` — optional one-liner naming the deliverable and the terminal signal that ends the run.

**In `PERMISSIONS_BLOCK.md`:**

1. Paste the whole block **near the top**, right after the agent's role sentence. Early tokens carry the most weight.
2. Fill the two slots:
   - `{ALLOWED_QUESTIONS}` — the only questions that workflow may ask (e.g. *"the intake message asking for script source, ratio and duration, and in the generate branch one genre message"*).
   - `{WORKFLOW_SPECIFIC_PRE_AUTHORIZED}` — optional; name that workflow's own controls so there is no doubt they are covered (e.g. *"Create New Audio, Generate Audio, Create Image, Create Video, Auto Align, Cut, Save, Export → Create"*).
3. Optionally paste the **FINAL REMINDER** (last lines of the file) at the very end of the prompt. First and last tokens are weighted most; the rules in between are easiest for a model to lose.

## What the run instruction block covers

| Failure | The reply it prevents |
|---|---|
| Prompt read as a document | "I received the prompt, but it doesn't include an actual topic or request — tell me what you'd like created." |
| Summarizing instead of starting | An outline or assessment of the workflow instead of its first step |
| Asking to begin | "Shall I start?" |
| Hunting for absent files | "Please attach the contract" when it is embedded in the same document |
| Restarting on Resume | Rebuilding completed work instead of reconciling and continuing |

## What the permissions block covers

Every category below came from an observed production failure, not from theory:

| Category | The ask it prevents |
|---|---|
| Action approval | "May I submit this?" · "May I click Generate?" |
| Credits / cost | "This will consume credits — do you confirm?" |
| Deletion | "Authorize deletion of the 9px tail fragment" |
| Editing | "Shall I cut the last clip?" |
| Saving | "May I save / overwrite?" |
| Navigation | "May I open a tab?" |
| Retries | "Should I retry the failed step?" |
| Export | stopping after save instead of publishing |
| Continuation | "Shall I continue?" · stopping at phase boundaries |
| Delegation | "Please open X and reply Resume" |

It also fixes the two framing errors that *cause* those asks: treating **credit consumption** as a purchase decision, and treating **working-state edits** as data loss.

## What it deliberately does NOT authorize

The block keeps a short list of genuine stop conditions, so an agent still halts where halting is right: login/CAPTCHA, an app-displayed refusal that blocks the action, an unrecoverable error after retries, an uncontrollable browser, a vanished job, anything destructive **outside** the workflow's scope (deleting saved work, changing account settings, spending money beyond normal generation, sending or publishing to third parties), and genuine ambiguity where any assumption could waste the run.

Never edit that list to make an agent "more autonomous" — it is what keeps the module safe to reuse everywhere.

## A caveat worth knowing

These rules make the expected behaviour unambiguous, which reliably helps capable models. They cannot manufacture stamina: a light-tier model asks permission and yields turns as a trained reflex, and no instruction can restart a turn that has already ended. For unattended runs, pair this module with a model tier that can sustain the work.

## Version

`v1.1` — run instruction block added. Distilled from the VOX documentary workflow contract (v3.4.0), August 2026.
