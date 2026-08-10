# Character World Travel

An end-to-end AI browser workflow for producing a consistent-character world-travel video with VideoExpress.ai and CloneVoice.ai.

## Files

- `SYSTEM_PROMPT.md` — reusable system prompt for Codex, ChatGPT, Claude, or another browser-capable AI agent.
- `WORKFLOW.json` — structured workflow configuration containing the VideoExpress clip-generation, timeline, CloneVoice music, audio-trimming, verification, recovery, and export steps.
- `README.md` — this usage guide.

## User inputs

The agent asks for exactly four inputs:

1. Locations, in the desired order.
2. Total video length.
3. Video ratio: Landscape (16:9) or Vertical (9:16).
4. A reference image or a character idea.

It then determines the story structure, number of clips, exact per-clip durations, image prompts, time-coded video prompts, music theme, CloneVoice settings, and export name. The selected ratio is preserved across the reference portrait, all prompts, generated media, project canvas, timeline, and export.

Clip durations are whole numbers from 3–10 seconds and must add up exactly to the requested total. For example, four locations over 30 seconds are planned as four clips lasting 8, 8, 7, and 7 seconds.

## Usage

1. Copy the complete contents of `SYSTEM_PROMPT.md` into the system-instruction field of a browser-capable AI automation environment.
2. Give the agent access to an existing authenticated browser session for VideoExpress.ai and CloneVoice.ai.
3. Answer the four intake questions.
4. Allow the agent to prepare the production brief and structured prompts, generate all clips, assemble the timeline, generate/import music, trim audio, and queue the final export.

The workflow stops for authentication, CAPTCHA, payment, missing access, or another unavoidable human-only action. It does not store credentials.

## Important workflow rules

- Attach the consistent character through **Reference Photo**. Never use **Use From Library**.
- Select Landscape 16:9 or Vertical 9:16 from the user’s answer and keep that ratio unchanged throughout the entire project.
- Keep the **Create Video From Prompt** modal open between clip submissions.
- Treat visible image-generation loading as an active job: continue polling automatically and never ask the user to reply “ready” only because previews are slow.
- The only successful terminal condition is the final VideoExpress export queue confirmation; pending generation must never end the automation turn.
- Use **Advanced Mode** and **Manual Video Length** for every clip.
- Add completed clips from **Media Library → My AI Videos** in planned scene order.
- Generate music with CloneVoice but do not download it.
- Import music through **Import Media Text to Speech → Import from CloneVoice.ai → Music**.
- Put the imported music on the lower timeline track at `00:00:00`.
- Cut the audio at the exact end of the final video clip and delete only the post-cut segment.
- Export as High-quality FullHD MP4 and verify the background queue confirmation.

## Completion condition

The workflow is complete when VideoExpress confirms that the exported movie has entered the background rendering queue. The agent’s final report records duration, scene order, music status, audio alignment, export settings, and queue confirmation.
