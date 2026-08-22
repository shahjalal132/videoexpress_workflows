# Nursery‑Rhyme Music Video Automation — CloneVoice → Artistly → VideoExpress

An autonomous, browser‑based agent workflow that turns **one idea + one ratio** into a finished nursery‑rhyme music video and submits it to VideoExpress's render queue — end to end, no manual clicking.

**Pipeline:** generate the song in **CloneVoice.ai** → build a character‑consistent storyboard in **Artistly.ai** → prepare `prompt_book.json` with one final prompt, image mapping, and duration per scene → generate clips with VideoExpress **Create Video From Prompt** → assemble the clips → add the music → **match the video endpoint exactly to the audio endpoint** → save → export FullHD MP4.

---

## 🎬 Example output (this exact workflow, verified run)

**Exported video:** https://cdn-ny-b.videoexpress.ai/video/1786603107_6a7d6663c966b.mp4

| | |
|---|---|
| Song | *Jannat and Her Toys* (CloneVoice, Kids‑Rhyme, English, 139.22 s) |
| Storyboard | 35 scenes, Artistly **Music Storyboard** agent, 16:9 |
| Video | 35 ordered 3D animation clips |
| Sync | video endpoint == audio endpoint, **0‑pixel difference** (final clip exact‑trimmed by 2.23 s) |
| Export | **Landscape 16:9 · High · FullHD · mp4** |

---

## ✍️ The prompt used

The agent asks exactly two questions, then runs autonomously. In this run the inputs were:

> **1. Idea/prompt:** `Bunny's ABC Garden. A cute bunny explores a magical garden where objects appear for each letter.`
>
> **2. Ratio:** `Landscape`

The protagonist and creative direction are resolved from the idea and used in lyrics and prompts. Generated storyboard content is accepted from structural metadata without visual or description-based character review.

Everything else — song title, lyrics, music style/language, protagonist identity lock, export name, story arc, and all visual treatment — was **inferred** from that one idea. You can add as much or as little detail to the idea as you like (protagonist gender/age/appearance/clothing, setting, action, mood, music style, language); anything omitted is inferred. **Only the ratio is never guessed.**

---

## 🚀 How to use

1. Make sure **CloneVoice.ai, Artistly.ai, and VideoExpress.ai are already signed in** in the browser the agent controls. (The workflow never handles credentials or API keys.)
2. Start the agent with the system prompt in [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) (or trigger phrase **"Generate nursery rhymes music video"**). Receiving the prompt starts the run; it is not a document-review request.
3. Answer the **two questions**: your idea, and **Landscape (16:9)** or **Vertical (9:16)**.
4. The agent presents a short production brief and then runs to completion on its own. Normal workflow actions are pre-authorized, so it does not ask for permission, credit confirmation, or a “ready”/“continue” response.
5. It finishes when VideoExpress shows **"Your movie creation is currently number N in the queue."** The MP4 appears under **My Videos** once background rendering completes.

Typical wall‑clock: a few minutes for the song, a few for the storyboard, then batched video generation (5 concurrent) — most of the time is the render queue, not interaction.

---

## 📁 Files in this folder

| File | Purpose |
|---|---|
| [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) | The agent's operating instructions. **§0** is the mandatory device‑agnostic interaction contract; **§17** is the exact DOM/API selector reference; **§18–19** cover validation checkpoints, support investigation, and the golden invariants. |
| [`WORKFLOW.json`](WORKFLOW.json) | Machine‑readable config (v2.8.2): verified audio transfer, structural-only storyboard acceptance, prompt-book preparation, current VideoExpress flow, checkpoints, recovery, and safety rules. |
| [`prompt_book.json`](prompt_book.json) | Runtime prompt-book template. The agent fills one ordered Artistly scene/Design ID/image mapping, final VideoExpress prompt, and 3–10 second Advanced Mode duration per scene before generation. |
| [`WORKFLOW_STATE.example.json`](WORKFLOW_STATE.example.json) | Compact v2.8.2 checkpoint example showing verified audio transfer, structural storyboard acceptance, prompt-book preparation, latest-flow submission, and separate generation monitoring. |
| `README.md` | This file. |

---

## 🔒 Key requirements & guarantees

- **Ratio is a project‑wide invariant** — every image, clip, canvas, and the export use the one chosen orientation; landscape and vertical are never mixed.
- **Identity lock at input/prompt level** — gender, age, hair, clothing, and visual medium are resolved once and repeated consistently in lyrics and prompts. Generated media is accepted without visual review.
- **Storyboard agent priority & fallback** — Artistly **Nursery Rhymes** runs first. Every attempt receives API-only structural validation: completed multi-scene batch, viable count, correct ratio, and sequential page numbers. Descriptions, names, prompts, thumbnails, identity, species, gender, theme, and appearance are never reviewed. Only structural failures count toward the three-attempt fallback to **Music Storyboard**; a user-approved Design-ID list is authoritative.
- **Narration-style, no lip-sync (v2.7.0)** — every VideoExpress prompt is sanitized and universally wrapped with narration/no-speech/close-mouth instructions, ending exactly with **`no mouth movement no lypsync`**. Prompt enhancement and lip sync remain off, and style is explicitly set to 3D. The prompt is checked once before submission; completed prompt metadata and mouth motion are not inspected.
- **Prompt book + latest VideoExpress flow (v2.8.0)** — prompts are prepared once in `prompt_book.json` against their Artistly Design IDs and imported image IDs. Generation uses **Create Video From Prompt**, Image Type 3D, both enhancement toggles off, Video Only on, Advanced Mode on, and the planned manual duration. The creator tab stays open; My AI Videos may be monitored in a separate tab without previewing clips.
- **Minimal validation for speed** — generated images and videos are never previewed, played, downloaded for review, screenshot, montaged, or frame-sampled. The first take is accepted from application completion signals and source-ID mapping. Validation is limited to acceptance, completion, structure, project-save persistence, and export confirmation.
- **Standing authorization** — navigation, named controls, generations, normal retries, working-timeline cleanup, save, and export are authorized when the run starts. The agent stops only for a true blocker such as login/CAPTCHA, visible app refusal, exhausted unrecoverable error, uncontrollable browser, vanished job, out-of-scope destruction, or unsafe ambiguity.
- **Verified audio handoff (v2.8.1)** — the exact completed CloneVoice CDN source is fetched and binary-validated before it is injected into Artistly FilePond. A Download click or guessed path is never accepted as proof. Local download is used only as a verified fallback, and source-transfer failures do not consume storyboard retry attempts.
- **Exact sync** — the final video endpoint equals the audio endpoint with **zero‑pixel tolerance** (achieved by the playhead‑set + cut + tail‑delete method).
- **`N` is dynamic** — however many storyboard scenes are produced, exactly that many timeline clips are made (no fixed count).
- **≤ 5 concurrent generations**, batched, with a completion barrier between batches.
- **Device‑agnostic** — actions use DOM selectors + JavaScript event dispatch (never screenshot pixel coordinates), so it works across screen sizes, zoom levels, and browsers.

---

## 🛠️ Resumability & support

The agent writes a durable **`WORKFLOW_STATE.json`** (see the `.example` file) after every verified side effect, with a per‑gate checkpoint `{gate, status, evidence, method, timestamp, artifact_ids}`. Evidence is signal-based (`api`, `dom`, or `document_title`), never generated-media inspection.

If a run fails and a user reports it, follow **`support_investigation`** in `WORKFLOW.json`:

1. Load `WORKFLOW_STATE.json`.
2. Find the first gate whose `status` isn't `pass`.
3. Read its `evidence` + surrounding `error_history`.
4. Re‑verify that gate live via the matching read API in `dom_api_contract` (auth, folder contents, job status, endpoints, queue) — app state is authoritative.
5. Resume from that gate's `next_safe_action` idempotently — never repeat a verified side effect.

Gates cover: **auth · CloneVoice · input/prompt identity · storyboard structure · import · batch submission · batch completion · no-speech pre-submit configuration · timeline · audio · sync · ratio · save · export.**

Known non‑blockers (do **not** stop): visible spinners/percentages, a stale success toast, normal credit consumption, phase boundaries, or an MCP/API `419` (use the browser DOM instead). Real blockers: login/CAPTCHA, a visible app refusal, an exhausted unrecoverable error, an uncontrollable session, a vanished job after recovery, an out-of-scope destructive action, or unsafe ambiguity.
