# PRACTICAL PROMPT GUIDE — Mouth-Locked AI Video Prompts

**Status: BINDING.** This document is the single authoritative method for writing every
`final_videoexpress_prompt` in this workflow. `SYSTEM_PROMPT.md` §3 and `WORKFLOW.json`
`inputs.videoexpress.prompt_architecture_version = "mouth_locked_best_practice_v1"` both resolve to
this guide. No other prompt style may be used, invented, or recalled from a previous run.

*A reusable method for preventing lip-sync while preserving expressive, cinematic motion.
General template + transformed examples. Designed for narration-style clips of any duration.*

---

## 1. Purpose and core principle

This guide converts ordinary character-animation prompts into **duration-independent** prompts that
strongly discourage speech-like mouth motion. The goal is **not** to freeze the whole character. It
is to lock only the **mouth, jaw, chin, and lower face** while allowing the eyes, eyebrows, head,
hands, clothing, posture, environment, and camera to remain alive.

> **Core principle** — Establish the closed-lip expression **once**, hold it **unchanged** for the
> remainder of the clip, and move all emotional performance into **non-mouth channels**.

### MASTER TEMPLATE

```
Single continuous shot. At the very beginning, [CHARACTER] gently brings the lips together into
a small closed-lip smile, then holds that expression perfectly unchanged for the remainder of the
clip. The lips stay sealed; the jaw, chin, cheeks, and lower face remain still, with no speaking,
lip-sync, mouth opening, or visible teeth.

[CHARACTER AND SCENE ACTION]. Emotion is shown only through soft blinks, eye and eyebrow movement,
subtle head motion, relaxed hand gestures, natural posture, clothing movement, and environmental
motion. [CAMERA, LIGHTING, STYLE, AND BACKGROUND DETAILS].
```

### Why this structure works

- It states a **positive mouth pose**: a small, sealed, closed-lip smile.
- It defines **when** the pose is established and **how long** it must remain unchanged.
- It locks the connected **lower-face anatomy**, not only the lips.
- It **redirects emotion** into safe motion channels, preventing a lifeless result.
- It avoids a **hard-coded duration**, so the prompt works with short or long clips.

---

## 2. Transformation method

1. Identify the subject, visual style, environment, action, camera behavior, and intended emotion in
   the raw prompt.
2. Move mouth control to the **opening sentences** so the model receives it as a primary constraint.
3. Describe **one brief settling action**: the lips gently come together into a small closed-lip smile.
4. **Lock the final state** for the remainder of the clip: sealed lips and a still jaw, chin, cheeks,
   and lower face.
5. Keep the original scene action, but assign expression to the eyes, eyebrows, head, hands, posture,
   clothing, and environment.
6. Retain camera, lighting, atmosphere, and animation style. Remove repetitive or misspelled negative tags.

### Recommended prompt order — ARCHITECTURE

```
1) shot continuity -> 2) mouth-set action -> 3) locked mouth state ->
4) character action -> 5) permitted expression -> 6) environment and camera
```

---

## 3. Mouth-control language

Strong prompts combine a clear **positive pose** with **concise exclusions**. Repeating every synonym
for speaking makes a prompt longer without making it more reliable.

| Use | Reason |
|---|---|
| `lips stay sealed` | Defines the visible pose directly. |
| `held perfectly unchanged` | Prevents the smile from drifting into speech shapes. |
| `jaw, chin, cheeks, and lower face remain still` | Suppresses secondary articulation that can look like dialogue. |
| `no speaking, lip-sync, mouth opening, or visible teeth` | Adds a compact negative safety boundary. |
| `emotion is shown only through…` | Preserves life and performance without using the mouth. |

### Avoid these weaknesses

- Using only "no lip-sync." The model may still create breathing, jaw, or smile changes.
- Contradicting "mouth closed for the entire clip" with a **later** instruction to close it. Say it
  settles closed at the beginning, then remains closed.
- Freezing the entire face. Preserve blinks, eye focus, eyebrows, and head movement.
- Hard-coding "five seconds" when clip duration may change.
- Overloading the ending with duplicated negatives such as "no talking, no singing, no chanting…".

---

## 4. Transformed example — Rafi at the rainy window

*Context: Rafi is a young 3D-animated boy in a yellow raincoat. The original scene depends on quiet
observation, rain movement, a hand touching glass, and a gentle camera move. The transformed prompt
protects those details while moving contentment into the eyes, head, hand, and posture.*

```
Single continuous 3D animated shot. At the very beginning, Rafi gently brings his lips together into
a small closed-lip smile, then holds it perfectly unchanged for the remainder of the clip. His lips
stay sealed; his jaw, chin, cheeks, and lower face remain still, with no speaking, lip-sync, mouth
opening, or visible teeth.

Wearing a bright yellow hooded raincoat and shiny red rain boots, Rafi slowly turns from the rainy
window toward the room, raises one hand, and softly presses it against the glass. His contentment
appears only through a soft blink, eye movement, a slight head turn, relaxed hand motion, and calm
posture. Rain droplets streak and splash across the window while a cozy interior remains softly
blurred behind him. Medium shot with subtle shifting light and a slow zoom out.
```

> **Transformation note** — The smile is established once. The head turn and hand movement remain
> active, but the lower face is explicitly excluded from the performance.

---

## 5. Transformed example — rain-watching close-up

*Context: This version keeps Rafi facing the window and uses a slow zoom in. The eager feeling is
carried by eye focus, eyebrows, blinks, and small posture changes instead of mouth animation.*

```
Single continuous 3D animated shot. At the very beginning, the boy settles into a gentle closed-lip
smile and holds it perfectly unchanged for the remainder of the clip. His lips remain sealed; his
jaw, chin, cheeks, and lower face stay still, with no speaking, lip-sync, mouth opening, or visible
teeth.

Wearing a yellow raincoat over a light blue shirt, grey pants, and red rain boots, he watches
droplets slide down a large window. His eagerness is expressed only through occasional blinks,
attentive eye focus, subtle eyebrow movement, a slight head tilt, and small posture shifts that
gently sway his raincoat. Soft green foliage blurs beyond the glass. Warm indoor lighting and a slow
camera zoom in.
```

---

## 6. Transformed example — Tom's sunset ride

*Context: Tom leans from a moving cab during sunset. The ride should feel relaxed and joyful, but the
character must read as listening to narration rather than delivering dialogue.*

```
Single continuous narration-style shot. At the very beginning, Tom gently brings his lips together
into a small closed-lip smile, then holds it perfectly unchanged for the remainder of the clip. His
lips stay sealed; his jaw, chin, cheeks, and lower face remain still, with no speaking, lip-sync,
mouth opening, or visible teeth.

Wearing his cap, Tom leans out of the cab and enjoys the sunset ride. His enjoyment appears only
through soft blinks, eye and eyebrow movement, a slight head tilt, relaxed hands, and natural body
posture. The moving cab, passing scenery, warm sunset light, and gentle clothing movement provide
continuous secondary motion.
```

---

## 7. Universal fill-in template

Replace the bracketed fields while **preserving the mouth-control sentences**. Add only the scene
details that materially affect the result.

### COPY-AND-ADAPT TEMPLATE

```
Single continuous [VISUAL STYLE] shot. At the very beginning, [CHARACTER] gently brings the lips
together into a small closed-lip smile, then holds it perfectly unchanged for the remainder of the
clip. The lips stay sealed; the jaw, chin, cheeks, and lower face remain still, with no speaking,
lip-sync, mouth opening, or visible teeth.

[WARDROBE / APPEARANCE]. [PRIMARY PHYSICAL ACTION]. [EMOTION] is expressed only through
[EYES / EYEBROWS / BLINKS / HEAD / HANDS / POSTURE]. [ENVIRONMENTAL MOTION].
[LIGHTING, LENS, FRAMING, AND CAMERA MOVE].
```

### Optional variations

- **Neutral expression:** replace "small closed-lip smile" with "relaxed neutral closed-mouth expression."
- **Already closed at frame one:** replace the settling action with "From the first frame, [CHARACTER] holds…".
- **Stricter control:** add "the mouth shape does not change during blinks, head turns, or body movement."
- **Multiple characters:** apply the mouth-lock instruction separately to each visible speaking-capable character.
- **Non-human characters:** name the relevant anatomy, such as muzzle, beak, mandible, or mouth seam.

---

## 8. Final quality checklist

Every item must pass before the prompt is stored or submitted.

1. The prompt begins with a single continuous shot and does not assume a fixed duration.
2. The mouth settles into one clearly described closed pose at the beginning.
3. The final mouth pose is held unchanged for the remainder of the clip.
4. Lips, jaw, chin, cheeks, and lower face are all controlled.
5. Speaking, lip-sync, mouth opening, and visible teeth are excluded concisely.
6. Emotion is reassigned to eyes, eyebrows, head, hands, posture, or body movement.
7. The original character, environment, action, camera, lighting, and style remain intact.
8. No instruction later in the prompt contradicts the mouth lock.
9. Redundant negative phrases and misspellings have been removed.

> **Fast rule** — If a viewer can understand the emotion with the mouth completely frozen, the prompt
> is structured correctly.

---

## 9. REJECTED reference — what a violation looks like

The following shape was produced by a real run (`Milo's Moonlight Train`, prompt book
`schema_version 1.0.0`) and is **invalid**. Recognize it and rebuild rather than reuse it.

```
Narration-style scene with no lip-sync: the character never speaks or mouths any words, and the lips
stay gently closed the entire time. In the first moments the character softly closes the mouth into a
gentle closed-lip smile and keeps it closed for the rest of the clip. <RAW ARTISTLY TEXT VERBATIM>
Mouth closed and lips together for the entire clip. The character never speaks, sings, talks, mouths
words, chants, or opens the mouth; no lip movement, no jaw movement, no visible teeth, no dialogue,
no singing, no lip sync. Emotion is expressed only through the eyes, eyebrows, head turns, hands, and
body movement. A gentle closed-lip smile is allowed. no mouth movement no lypsync
```

Why it fails, item by item:

| Defect | Guide rule broken |
|---|---|
| Generic prefix + raw text + generic suffix — a **wrapper**, not a transformation | §2 steps 1–6 |
| A `no_speech_constants` block of reusable prefix/suffix/tail strings | §2 — prompts are transformed per scene, never assembled from constants |
| Raw Artistly text left untouched in the middle, so later cues survive: `warm, inviting smile`, `determined, joyful expression`, `wearing a gentle closed-lip smile`, `animatedly pointing`, `enthusiastically counting` | §8 item 8, §3 "Avoid these weaknesses" |
| Duplicated negative pile-up at the end | §3 "Avoid these weaknesses" |
| Misspelled terminal tail `no mouth movement no lypsync` | §8 item 9 |
| Mouth control stated at both ends instead of once at the opening | §3 "Avoid these weaknesses" |
| Identity paragraph repeated verbatim in every scene after the action | §7 — identity belongs in `[WARDROBE / APPEARANCE]` inside the second block |

**A prompt book containing `no_speech_constants`, a `schema_version` below `1.1.0`, or any string
from this section is void. Discard it and rebuild every scene from §7 before generating anything.**
