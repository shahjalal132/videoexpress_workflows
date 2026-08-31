# Ghibli ASMR — VideoExpress Browser Workflow

This folder contains a resumable, end-to-end browser-automation workflow that turns one user idea and requested duration into a character-consistent anime ASMR short. The clip count is calculated from the duration rather than fixed. The visual direction uses nostalgic hand-painted Japanese-animation warmth, while the sound design is derived from the requested story and emphasizes synchronized ambience and satisfying environmental SFX.

Every new run begins with exactly three questions: **Idea/Prompt**, **Ratio (Landscape or Vertical)**, and **Duration**. Duration may be supplied as seconds, `M:SS`, or natural language such as “2 minutes,” with a hard maximum of 5 minutes. After the user answers, the agent calculates the required clips and writes a fresh [`prompt_book.json`](prompt_book.json) containing the resolved title, an immutable character bible (including exact facial-hair state), character and world locks, sequential image prompts, timed image-to-video prompts, per-clip durations, background ambience, and synchronized SFX. The checked-in Roadside Tea Stop prompt book is a complete example and is replaced for a new idea.

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
4. Generate and validate a continuous `prompt_book.json` with exactly the calculated number of scenes, an immutable character bible, verbatim positive and negative identity locks in every prompt, a `world_lock` and `lighting_lock` pasted verbatim into every image prompt, per-scene `start_state`/`end_state` fields forming an unbroken handoff chain (each clip begins exactly where the previous one ended), and physically grounded action beats — stable supported poses in every still, no walking on water or mid-mount stills, and frame-relative directions whose landing points agree with the action.
5. Propagate the selected ratio and duration plan to global settings, every image composition, the VideoExpress creation modal, project canvas, timeline, and export.
6. Keep one **creator tab** open with the configured Create Video from Prompt panel.
7. Generate one master portrait for each recurring character, record its stable reference ID, and reuse that exact reference for every scene. Never chain a previous scene image as the next scene's identity reference.
8. For each scene, select the chosen ratio, attach the same character reference, paste the scene's image prompt, select **2D**, disable automatic image-prompt enhancement, and create the image.
9. Run the narrow identity gate: compare the generated still with the master reference for face geometry, age, skin tone, eyes, hair, exact facial-hair state, body proportions, and wardrobe. Reject unexpected moustaches/mustaches, beards, stubble, age changes, or any other identity drift.
10. Against the identity-approved image, paste the matching image-to-video prompt, open **Advanced Mode**, disable video-prompt enhancement, set that scene's calculated manual duration, retain generated audio, and create the video.
11. Use up to five concurrent generation slots in batches. Monitor jobs from a second tab and identify every result by its prompt or stable media ID, never by grid position alone. Check only the opening, midpoint, and closing frames for identity drift before accepting a video.
12. After all calculated jobs complete, right-click each clip and choose **Add to Timeline** in scene order 1→N.
13. Click **Auto Align Clips** only after every calculated clip is present, then verify count, order, total duration, and contiguous geometry.
14. Save and export using the generated prompt-book title, with **High / FullHD / MP4**, and persist the queue confirmation.

The workflow uses semantic DOM roles, labels, stable attributes, and visible text. It does not depend on screenshot coordinates, display scaling, or browser zoom. It never aesthetically grades generated media. Character identity is the one mandatory visual exception: each still and three sampled video frames are checked only for immutable identity traits, especially facial hair, before acceptance.

## Running it

Give the contents of `SYSTEM_PROMPT.md` to an agent with browser-control capability and access to this folder. Receiving that system prompt starts the run. Its first response must ask only:

1. **Idea/Prompt:** What should the video be about?
2. **Ratio:** Landscape or Vertical?
3. **Duration:** How long should the finished video be? Maximum 5 minutes.

The three questions should be sent together. After the answer, the agent calculates the clip plan, writes the prompt book, and continues without another routine confirmation. The intake answers are the approval for the whole run, including generation quota: the agent must not ask before submitting generations, must not stop while videos process (it polls until they complete), and must run in one uninterrupted turn from intake to the export queue confirmation. "Resume" is only for genuinely interrupted sessions, never a routine second half of a run.

Note: prompt rules govern behavior within a turn but cannot stop a model from ending its turn early. In live tests, GPT-5.6 Luna Light and Terra Medium still yielded mid-run despite standing authorization; for unattended completion use Sol Medium or higher (or drive the run with an external auto-continue loop).

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
- every recurring character has one immutable character bible and one stable master-reference ID reused across all of that character's scenes;
- every scene still and the opening/midpoint/closing frames of every video pass the identity gate, including exact facial-hair state;
- one distinct completed video ID is mapped to every scene 1–N;
- timeline count equals N, scene order is verified, and its planned duration matches the request;
- `Auto Align Clips` was clicked after the final clip was added and the resulting geometry is contiguous;
- the named project is saved;
- the FullHD MP4 export is submitted and the exact queue confirmation text is recorded.
