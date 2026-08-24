# Ghibli ASMR — VideoExpress Browser Workflow

This folder contains a resumable, end-to-end browser-automation workflow that turns one user idea and requested duration into a character-consistent anime ASMR short. The clip count is calculated from the duration rather than fixed. The visual direction uses nostalgic hand-painted Japanese-animation warmth, while the sound design is derived from the requested story and emphasizes synchronized ambience and satisfying environmental SFX.

Every new run begins with exactly three questions: **Idea/Prompt**, **Ratio (Landscape or Vertical)**, and **Duration**. Duration may be supplied as seconds, `M:SS`, or natural language such as “2 minutes,” with a hard maximum of 5 minutes. After the user answers, the agent calculates the required clips and writes a fresh [`prompt_book.json`](prompt_book.json) containing the resolved title, character and world locks, sequential image prompts, timed image-to-video prompts, per-clip durations, background ambience, and synchronized SFX. The checked-in Roadside Tea Stop prompt book is a complete example and is replaced for a new idea.

## Files

- `prompt_book.json` — generated source of truth for the current run; initially contains a complete example.
- `SYSTEM_PROMPT.md` — operating instructions for an autonomous browser agent such as Codex or Claude.
- `WORKFLOW_STATE.json` — durable checkpoint state for fresh runs and exact resume behavior.
- `README.md` — workflow overview and operator handoff.
- `../common_permissions/README.md` — repository-wide authorization and minimal-validation policy incorporated into this workflow.

## End-to-end browser automation

The agent controls an already signed-in VideoExpress browser session and performs the complete production flow:

1. Ask the user, in one intake message, for **Idea/Prompt**, **Ratio: Landscape or Vertical**, and **Duration**.
2. Normalize Landscape to 16:9 and Vertical to 9:16. Parse duration into whole seconds and reject values above 300 seconds.
3. Calculate `clip_count = ceil(total_seconds / 10)`. Distribute the seconds as evenly as possible so every clip is an integer duration no longer than 10 seconds and the sum equals the requested duration.
4. Generate and validate a continuous `prompt_book.json` with exactly the calculated number of scenes.
5. Propagate the selected ratio and duration plan to global settings, every image composition, the VideoExpress creation modal, project canvas, timeline, and export.
6. Keep one **creator tab** open with the configured Create Video from Prompt panel.
7. For each scene, select the chosen ratio, paste the scene's image prompt, select **2D**, disable automatic image-prompt enhancement, and create the image.
8. Against that exact image, paste the matching image-to-video prompt, open **Advanced Mode**, disable video-prompt enhancement, set that scene's calculated manual duration, retain generated audio, and create the video.
9. Use up to five concurrent generation slots in batches. Monitor jobs from a second tab and identify every result by its prompt or stable media ID, never by grid position alone.
10. After all calculated jobs complete, right-click each clip and choose **Add to Timeline** in scene order 1→N.
11. Click **Auto Align Clips** only after every calculated clip is present, then verify count, order, total duration, and contiguous geometry.
12. Save and export using the generated prompt-book title, with **High / FullHD / MP4**, and persist the queue confirmation.

The workflow uses semantic DOM roles, labels, stable attributes, and visible text. It does not depend on screenshot coordinates, display scaling, or browser zoom. Generated media is accepted from the application's completion signal; the agent does not preview or aesthetically grade its own output unless the user explicitly requests a review.

## Running it

Give the contents of `SYSTEM_PROMPT.md` to an agent with browser-control capability and access to this folder. Receiving that system prompt starts the run. Its first response must ask only:

1. **Idea/Prompt:** What should the video be about?
2. **Ratio:** Landscape or Vertical?
3. **Duration:** How long should the finished video be? Maximum 5 minutes.

The three questions should be sent together. After the answer, the agent calculates the clip plan, writes the prompt book, and continues without another routine confirmation.

Example: a 2-minute answer is 120 seconds. With VideoExpress limited to 10 seconds per generated clip, the default plan is 12 clips × 10 seconds. For 121 seconds, the balanced plan is 13 clips: four clips × 10 seconds and nine clips × 9 seconds. The planner may use more shorter clips when the story clearly needs faster pacing, but it may never exceed 10 seconds per clip or the requested total.

Prerequisites:

- VideoExpress is already signed in in the controllable browser.
- The account supports five concurrent generations.
- The browser agent can click, type, inspect DOM state, open a monitoring tab, and retain the creator tab.
- The repository files are writable so `WORKFLOW_STATE.json` can be checkpointed after verified side effects.

If a run is interrupted, tell the agent **Resume**. It must read `WORKFLOW_STATE.json`, reconcile recorded IDs and statuses against the live application, and continue from `next_safe_action` without regenerating completed assets.

## Authorization and safety

The run inherits the common policy in [`../common_permissions/README.md`](../common_permissions/README.md): ordinary workflow actions—navigation, generation, retries, timeline editing, saving, and export—are pre-authorized. The agent does not ask routine permission or stop at phase boundaries.

This does not authorize credential handling, CAPTCHA bypass, deleting saved projects or library media, changing account settings, purchasing beyond normal generation, or publishing to third parties. Those remain true blockers or out-of-scope actions.

## Completion signal

The workflow is complete only when all of the following are persisted in `WORKFLOW_STATE.json`:

- the intake idea, normalized ratio, requested duration, calculated clip count, and duration plan are persisted;
- the generated prompt book contains the calculated scene count and its scene durations sum to the requested duration;
- one distinct completed video ID is mapped to every scene 1–N;
- timeline count equals N, scene order is verified, and its planned duration matches the request;
- `Auto Align Clips` was clicked after the final clip was added and the resulting geometry is contiguous;
- the named project is saved;
- the FullHD MP4 export is submitted and the exact queue confirmation text is recorded.
