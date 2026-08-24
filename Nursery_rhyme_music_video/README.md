# Nursery-Rhyme Music Video Automation

**CloneVoice → Artistly → VideoExpress.** An autonomous browser agent that turns **one idea + one ratio** into a finished nursery-rhyme music video and submits it to the render queue.

---

## Summary

You answer two questions — the song idea and Landscape or Vertical. The agent does the rest:

1. writes and generates the song in **CloneVoice.ai**;
2. builds a character-consistent storyboard in **Artistly.ai** from that song;
3. prepares `prompt_book.json` — one final prompt, image mapping, and duration per scene;
4. generates one **VideoExpress.ai** clip per storyboard scene (5 at a time);
5. assembles the clips, adds the music, and trims the video endpoint to match the audio exactly;
6. saves the project and submits the FullHD MP4 export.

It runs to completion without check-ins. Everything else — title, lyrics, style, language, protagonist, story arc, export name — is inferred from your idea. **Only the ratio is never guessed.**

**Example output:** https://drive.google.com/file/d/1AqmNDlgVLYsVKFcR9QupiO40vaZy_h3j/view?usp=sharing

---

## Requirements

- **Accounts signed in** at CloneVoice.ai, Artistly.ai, and VideoExpress.ai, in the browser the agent controls. The workflow never handles credentials or API keys.
- **Generation credits** on all three. Credit use is expected and the agent will not stop to ask about it.
- **An agent runtime that can drive a browser** — it acts through DOM selectors and JavaScript events, never screenshot coordinates, so it works at any screen size or zoom.
- **A VideoExpress plan allowing 5 concurrent generations** (the batch size the workflow assumes).
- The files in this folder. `SYSTEM_PROMPT.md` is the whole contract; the rest are its references and runtime state.

---

## Agent model — best performance

**Use GPT-5.6 Sol at Medium reasoning effort.** This is the tested configuration.

GPT-5.6 has two independent dials: the **capability tier** (Luna → Terra → Sol) and the **reasoning effort** (Light → Medium → High → Extra High). They do not substitute for each other — Terra at High reasoning is not Sol.

What matters here is not raw problem-solving but **stamina and instruction adherence**. A single run is 100+ tool calls over 30–60 minutes across three web apps. Lighter tiers have been observed to stall rather than fail: they ask permission mid-run, hand browser clicks back to the user, yield the turn at each phase boundary, and drop the browser session between turns. The run still converges through the checkpoint/`Resume` mechanism, but it needs constant nudging.

**Sol + Medium** completes a run unattended. Higher effort is fine; lower tiers are not recommended.

If a run does stop early, say **`Resume`** — the agent reloads `WORKFLOW_STATE.json`, reconciles it against the live apps, and continues from the smallest missing action without repeating finished work.

---

## Steps

1. Sign in to CloneVoice.ai, Artistly.ai, and VideoExpress.ai in the agent's browser.
2. Start the agent with the contents of [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) (or the trigger phrase **"Generate nursery rhymes music video"**). Receiving the prompt starts the run — it is not a document to review.
3. Answer the two questions:
   - **Idea/prompt** — the song and story. Add as much or as little as you like: protagonist, gender, age, appearance, clothing, setting, action, mood, music style, language. Anything omitted is inferred.
   - **Ratio** — **Landscape (16:9)** or **Vertical (9:16)**.
4. The agent presents a short production brief and then runs on its own. It reports progress but does not ask for approval to generate, import, edit the timeline, save, or export.
5. It finishes when VideoExpress shows **"Your movie creation is currently number N in the queue."**
6. Collect the MP4 under **My Videos** once background rendering completes.

Typical wall-clock: a few minutes for the song, a few for the storyboard, then batched clip generation. Most of the time is render queue, not interaction.

The agent stops early only for a real blocker: login or CAPTCHA, a visible app refusal, an unrecoverable error, an uncontrollable browser, or a job that vanished and stayed missing.

---

## Key rules the agent enforces

- **Prompt guide is binding.** Every video prompt is written from [`PROMPT_GUIDE.md`](PROMPT_GUIDE.md) — the mouth-locked method that keeps characters from lip-syncing while staying expressive. No other prompt style is permitted, and every scene must pass a nine-item check before it can be generated.
- **Ratio is project-wide.** Every image, clip, canvas, and the export use the one chosen orientation. Orientations are never mixed.
- **Scene count is dynamic.** However many storyboard scenes Artistly produces, exactly that many clips are made.
- **Exact sync.** The final video endpoint matches the audio endpoint to zero pixels.
- **First take ships.** Generated images and videos are never previewed or judged for looks; only structure, counts, IDs, and completion are checked. This is what keeps the run fast.

---

## Files

| File | Purpose |
|---|---|
| [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) | The agent's full operating contract. Start the agent with this. |
| [`PROMPT_GUIDE.md`](PROMPT_GUIDE.md) | The binding mouth-locked prompt method — template, checklist, and a rejected example. |
| [`WORKFLOW.json`](WORKFLOW.json) | Machine-readable config (v2.10.0) mirroring the system prompt. |
| [`prompt_book.json`](prompt_book.json) | Runtime template: one scene mapping, final prompt, and duration per storyboard scene. |
| [`WORKFLOW_STATE.json`](WORKFLOW_STATE.json) | Live run checkpoint — written after every verified step, and what `Resume` reads. |
| [`WORKFLOW_STATE.example.json`](WORKFLOW_STATE.example.json) | A filled-in checkpoint example. |
