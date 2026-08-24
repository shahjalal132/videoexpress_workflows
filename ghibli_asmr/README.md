# Ghibli ASMR — VideoExpress Browser Workflow

This folder contains a resumable, end-to-end browser-automation workflow that turns one user idea into a five-clip, character-consistent anime ASMR short. The visual direction uses nostalgic hand-painted Japanese-animation warmth, while the sound design is derived from the requested story and emphasizes synchronized ambience and satisfying environmental SFX.

Every new run begins with exactly two questions: **Idea/Prompt** and **Ratio (Landscape or Vertical)**. After the user answers, the agent writes a fresh [`prompt_book.json`](prompt_book.json) containing the resolved title, character and world locks, five sequential image prompts, timed image-to-video prompts, durations, background ambience, and synchronized SFX. The checked-in Roadside Tea Stop prompt book is a complete example and is replaced for a new idea.

## Files

- `prompt_book.json` — generated source of truth for the current run; initially contains a complete example.
- `SYSTEM_PROMPT.md` — operating instructions for an autonomous browser agent such as Codex or Claude.
- `WORKFLOW_STATE.json` — durable checkpoint state for fresh runs and exact resume behavior.
- `README.md` — workflow overview and operator handoff.
- `../common_permissions/README.md` — repository-wide authorization and minimal-validation policy incorporated into this workflow.

## End-to-end browser automation

The agent controls an already signed-in VideoExpress browser session and performs the complete production flow:

1. Ask the user, in one intake message, for **Idea/Prompt** and **Ratio: Landscape or Vertical**.
2. Normalize Landscape to 16:9 and Vertical to 9:16, then generate and validate a new five-scene `prompt_book.json` from the idea.
3. Propagate the selected ratio to global settings, every image composition, the VideoExpress creation modal, project canvas, and export.
4. Keep one **creator tab** open with the configured Create Video from Prompt panel.
5. For each scene, select the chosen ratio, paste the scene's image prompt, select **2D**, disable automatic image-prompt enhancement, and create the image.
6. Against that exact image, paste the matching image-to-video prompt, open **Advanced Mode**, disable video-prompt enhancement, set the planned manual duration, retain generated audio, and create the video.
7. Use up to five concurrent generation slots. Monitor jobs from a second tab and identify every result by its prompt or stable media ID, never by grid position alone.
8. After all five jobs complete, right-click each clip and choose **Add to Timeline** in scene order 1→5.
9. Click **Auto Align Clips** only after every clip is present, then verify count, order, and contiguous geometry.
10. Save and export using the generated prompt-book title, with **High / FullHD / MP4**, and persist the queue confirmation.

The workflow uses semantic DOM roles, labels, stable attributes, and visible text. It does not depend on screenshot coordinates, display scaling, or browser zoom. Generated media is accepted from the application's completion signal; the agent does not preview or aesthetically grade its own output unless the user explicitly requests a review.

## Running it

Give the contents of `SYSTEM_PROMPT.md` to an agent with browser-control capability and access to this folder. Receiving that system prompt starts the run. Its first response must ask only:

1. **Idea/Prompt:** What should the video be about?
2. **Ratio:** Landscape or Vertical?

The two questions should be sent together. After the answer, the agent writes the prompt book and continues without another routine confirmation.

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

- the intake idea and normalized ratio are persisted, and the generated prompt book passes schema checks;
- five distinct completed video IDs mapped to scenes 1–5;
- timeline count is five and order is verified;
- `Auto Align Clips` was clicked after the final clip was added and the resulting geometry is contiguous;
- the named project is saved;
- the FullHD MP4 export is submitted and the exact queue confirmation text is recorded.
