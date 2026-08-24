# Ghibli ASMR — VideoExpress Browser Workflow

This folder contains a resumable, end-to-end browser-automation workflow for producing **Roadside Tea Stop**, a five-clip, character-consistent 16:9 anime short. A young South Asian rider travels on the same classic black motorcycle, notices the same roadside tea stall, parks, receives tea, and quietly enjoys it. The visual direction uses nostalgic hand-painted Japanese-animation warmth; the sound design emphasizes motorcycle texture, countryside ambience, kettle steam, pouring tea, glass contact, leaves, birds, and other gentle ASMR details.

The canonical creative specification is [`prompt_book.json`](prompt_book.json). It contains the character lock, motorcycle and location continuity, image prompt, timed image-to-video prompt, duration, background ambience, and synchronized SFX for every scene.

## Files

- `prompt_book.json` — source of truth for all five scenes and generation settings.
- `SYSTEM_PROMPT.md` — operating instructions for an autonomous browser agent such as Codex or Claude.
- `WORKFLOW_STATE.json` — durable checkpoint state for fresh runs and exact resume behavior.
- `README.md` — workflow overview and operator handoff.
- `../common_permissions/README.md` — repository-wide authorization and minimal-validation policy incorporated into this workflow.

## End-to-end browser automation

The agent controls an already signed-in VideoExpress browser session and performs the complete production flow:

1. Load and validate `prompt_book.json` and `WORKFLOW_STATE.json`.
2. Keep one **creator tab** open with the configured Create Video from Prompt panel.
3. For each scene, choose 16:9, paste the scene's image prompt, select **2D**, disable automatic image-prompt enhancement, and create the image.
4. Against that exact image, paste the matching image-to-video prompt, open **Advanced Mode**, disable video-prompt enhancement, set the manual duration to 7 seconds, retain generated audio, and create the video.
5. Use up to five concurrent generation slots. Monitor jobs from a second tab and identify every result by its prompt or stable media ID, never by grid position alone.
6. After all five jobs complete, right-click each clip and choose **Add to Timeline** in scene order 1→5.
7. Click **Auto Align Clips** only after every clip is present, then verify count, order, and contiguous geometry.
8. Save the project as `Roadside Tea Stop - Consistent Anime Story`.
9. Export `Roadside Tea Stop - Consistent Anime Story` as **High / FullHD / MP4** and persist the queue confirmation.

The workflow uses semantic DOM roles, labels, stable attributes, and visible text. It does not depend on screenshot coordinates, display scaling, or browser zoom. Generated media is accepted from the application's completion signal; the agent does not preview or aesthetically grade its own output unless the user explicitly requests a review.

## Running it

Give the contents of `SYSTEM_PROMPT.md` to an agent with browser-control capability and access to this folder. Receiving that system prompt starts the run. The agent should not summarize the file or ask whether to begin.

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

- five distinct completed video IDs mapped to scenes 1–5;
- timeline count is five and order is verified;
- `Auto Align Clips` was clicked after the final clip was added and the resulting geometry is contiguous;
- the named project is saved;
- the FullHD MP4 export is submitted and the exact queue confirmation text is recorded.
