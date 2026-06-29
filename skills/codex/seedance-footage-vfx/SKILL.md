---
name: seedance-footage-vfx
description: >
  Write, improve, or rewrite Seedance 2.0 prompts that TRANSFORM footage the user already has
  (video-to-video), rather than building a scene from scratch. Use whenever a real clip is the
  starting point and they want a Seedance prompt to: add a VFX element (set a head or hair on fire,
  transform a hand, make a limb invisible), swap the environment around a preserved subject (clouds,
  lava, a neon city), drop a giant photoreal creature onto a
  landmark, relight or regrade so the subject and added elements look like one shot, sync a crash
  zoom or push-in to a spoken line or timecode, or generate a matching transformed start frame to
  animate from. Also use when they paste such a prompt and ask to change its lighting, timing,
  creature, or runtime. Trigger even if they just say "make a Seedance prompt for this video" with a
  clip attached. This is the video-to-video specialization; for a brand-new scene from image
  references with no source clip to preserve, the general Seedance prompter applies instead.
---

# Seedance 2.0 — Footage Transformation

Algoticlab Codex adaptation. Use this skill in Codex when the user is transforming existing footage with Seedance 2.0; preserve the original clip and write copy-ready English prompts following the rules below.

This skill is for **editing a clip the user already has**: keep a real subject and the real camera
move, change only what they ask for. It is the video-to-video sibling of the general Seedance
prompter. It reuses the same prompt grammar — a compact specs line, scene action written as a
single continuous shot (the source is one take), an `SFX` line at the end, and a terse director's
voice — and adds the transformation layer below. If the `seedance-prompter-v2` skill is also
available, reuse its grammar and style verbatim; do not contradict it.

## The core idea: preserve, then change one thing

A transformation prompt has two jobs that pull against each other: **lock** everything that makes
the source recognizable (the person's identity, face, wardrobe, performance, framing, lens, and
camera motion), and **change** only the named element. If you under-specify the lock, Seedance
re-rolls the face or the camera and the edit stops matching the original. So every prompt states
both halves explicitly, and repeats the most fragile guardrail — usually **"face and identity
unchanged"** — at the end of the action.

## Prompt anatomy (transform variant)

### 1. The `@source` declaration

The source clip is the base, not a style reference. One line:

```
@source: Original <clip name> — <who/what is in it: subject, wardrobe, setting, action>. Preserve
<identity, face, wardrobe, performance, framing, camera and motion> exactly; <what to change —
e.g. enhance only the environment / add the creature on the tower / transform only the right arm>.
```

If a transformation needs a real texture the model keeps faking (an animal's fur, a specific face),
add a **second input** as a texture reference and declare it:

```
@creature: Reference photo of a real <animal> — <fur / face / anatomy notes>. Appearance and
fur/skin texture reference only; ignore the photo's background and lighting, do not use it for the
environment.
```

The user supplies their own descriptions of what is in their files — use the tags correctly, don't
invent what the clip contains. But before writing `@source` for a clip you can open, **inspect it**:
read its duration / fps / aspect and extract a few frames. Build `@source` and the specs runtime from
what the footage actually shows — subject, wardrobe, framing, camera move, time of day, key direction —
not from the user's one-line summary. Set the specs duration to the probed runtime by default. If no
source clip is described, ask what footage they're starting from before writing.

### 2. Specs line

One compact line. Always include the source-matching constraints:

```
Photoreal. <aspect, default 16:9>. <duration — match the source clip>s. <look / grade>. NON-IP —
generic <creature/design>, not based on any brand or character. <SFX only | SFX and source
dialogue only>.
```

- **Match the source runtime by default.** If the clip is 6s, the prompt is 6s. Extend only when a
  payoff needs room (a slow creature turning to camera), and say why.
- **NON-IP guardrail** belongs in the specs line whenever a creature, armor, vehicle, or character
  design is added — generic, never a branded character. This keeps outputs clean and tends to
  generate more reliably than a trademarked design.
- **Audio:** `SFX only` for added effects; `SFX and source dialogue only` when the source talk
  track must survive (e.g. a zoom synced to a spoken line — see timing reference).

### 3. Scene action — one continuous shot

The source is a single take, so describe **continuous camera movement**, not cuts. Lead with the
shot/lens and "same framing as the source," then the preserved performance, then the transformation,
then any timed camera move. Close with the lock-down clause.

### 4. SFX line

End with a specific, ordered SFX note, exactly as the general grammar requires. For added effects be
behavioral: not "fire" but "a soft whoomph as it catches, then a low steady flame roar and crackle,
occasional ember pop."

---

## Two transformation modes

### A. Add an element to the footage

Set a head on fire, transform a hand, make a limb invisible, perch a creature on a landmark. Keep
the whole plate; layer the effect in.

- Describe the effect's **physics and behavior over time**, not just its presence: where it starts,
  how it spreads, how it moves, what light it throws. We used directional "creep" for transformations
  ("starts at the tattoo, fine seams split one at a time and peel back, a servo seats, a cable plugs
  in, the next plate locks…") and ignition-then-build for fire.
- Make the effect **interact with the plate**: firelight flickering on a face and spilling onto a
  car's paint; a glassy invisible arm refracting the background; a giant creature casting a real
  soft-edged contact shadow on the structure it grips.
- **Scale must be explicit** for giant creatures, or the model renders them life-size. Say
  "enormous, its massive body dwarfing the structure, clearly colossal relative to the mast."
- The subject usually stays **oblivious / unfazed**, mid-delivery — that contrast is the joke. State it.

### B. Replace the environment around a preserved subject

Keep the person, their vehicle, the seatbelt, the camera rig and its move; swap the whole world.

- The new world must **stream past with parallax** consistent with the original motion. If the car
  was driving, the replacement must give it a surface to drive on and things that rush past at speed.
- **Warm, directional daylight worlds are safer** for face/identity consistency than night or neon —
  those force a full relight of the subject and raise drift risk. Flag this tradeoff and bake the
  relight instruction in when the user wants night/neon anyway.

---

## Lighting integration (the part that makes or breaks it)

First decide the fork with the user — it changes everything:

- **Preserve the subject's lighting, grade only the new elements.** Lock the subject's original
  light; light and grade the added creature/environment to match the existing key on the subject so
  they integrate. Lowest identity risk.
- **Relight the whole frame under one look.** Subject included. Use this for a unified cinematic /
  commercial grade. Higher risk to the face, so keep identity/expression/wardrobe explicitly locked
  while only lighting and grade change.

Color matching alone is **not enough** to make a preserved subject sit in a new world — that's the
most common "looks pasted in" failure. When integrating a subject (or a creature) into a plate, go
beyond color with this recipe:

- **Light:** same key direction (name it — screen-left or screen-right), same softness, same shadow
  density and direction across the subject.
- **Environmental bounce:** let the world spill onto the subject — cool skylight from above, a warm
  bounce from sunlit ground/foliage, subtle ambient occlusion where forms meet.
- **Optics & atmosphere:** match lens character and micro-contrast; add a touch of the scene's
  atmospheric haze over the subject so they aren't unnaturally crisp against a hazy background; match
  depth of field, focus falloff and film grain to the rest of the frame.
- **Edges & grounding:** remove hard cut-out edges, halos and mismatched rims; ground the subject
  with believable depth so they occupy the same space.

State the time of day and key direction concretely ("soft, diffused midday daylight with the key
coming from screen-right"). "Softer" means a larger, more diffuse source: gentle soft-edged shadows,
low contrast, smooth highlight rolloff, light haze.

## Photoreal creature / element integration

When a creature or hard-surface element is added and must read as real:

- Demand wildlife-documentary / practical realism explicitly: "fully photoreal, real fur with depth
  and individual strands (or true scale detail / brushed metal), true anatomy, **never CG, plastic or
  cartoonish**."
- Tie it into the plate: same sun direction and color temperature as the subject, real soft-edged
  contact shadow on what it touches, same hazy atmosphere and depth as the far background.
- If it still reads as CG after a take, the reliable fix is a **second input** — a reference photo of
  the real animal/material — declared as a texture-only reference (see `@creature` above). Rewrite the
  prompt to point the creature at it.
- Behavior must match the species: a sloth shifts slow heavy weight; a chimp is alert and twitchy; a
  snake's coils tighten and a forked tongue tastes the air (and snakes don't blink — use an unblinking
  stare, not a blink, for a reptile payoff).

## Timed camera moves synced to dialogue

A crash zoom or smooth push-in landing on a beat is a recurring payoff. Anchor it **two ways at once**
so it lands even if Seedance's internal timing drifts: a semantic cue and a numeric cue.

- Semantic: `On the line "<exact words>," the camera <snaps into a hard crash zoom | begins a smooth,
  steady push-in> …`. Requires `SFX and source dialogue only` in the specs so the talk track survives.
- Numeric: `At about <T> seconds … the camera …`. Get `T` from the source audio — **see
  `references/dialogue-timing.md`** for how to measure it and convert a timecode.
- **Crash zoom** = fast hard punch-in; **smooth push-in** = slow steady glide, no snap. Match the
  user's word.
- If a landmark or subject must stay visible **through** the move, say so explicitly ("the tower stays
  in frame throughout, never cropped") — the camera pushes toward the element on the landmark, keeping
  the landmark in shot.
- Leave enough tail after the trigger for the payoff to play (a creature slowly turning to camera needs
  ~2–3s). If the clip is short, fire the zoom on the first word of the line rather than after it.

### Reveal pull-back (the outward move)

The mirror of the push-in: open tight on the *added* element in isolation — a long-telephoto, compressed
framing of the creature/effect with the subject out of frame — then move outward to land on the real
plate. Two flavors, match the user's word:

- **Hard / snap zoom-out** = a fast punch outward, abrupt. Use "snaps a hard, fast zoom-out."
- **Smooth pull-back** = a slow steady decompression, no snap.

Anchor the landing the same two ways as a timed zoom — semantic ("lands on the original framing") and
numeric ("at about T seconds"). Critically, demand a **100% match of the source composition** at the
landing: name the matched attributes — same angle, headroom, horizon, lens character — or the model
lands on a near-miss framing that no longer cuts against the original. After the landing, hand off to the
preserved take and keep the source's own camera motion running.

### Preserving lip-sync to a known line

When the payoff is the subject's mouth matching a specific line, quote it **verbatim** and anchor it
twice: once inside the action ("…lips matching the source exactly, saying clearly: '<line>'…") and once
in the SFX/dialogue line. Require `SFX and source dialogue only` in the specs so the talk track survives,
and add "lips matching the source exactly" to the lock-down clause. Then check the line against the
surviving dialogue window (see **Prepended-intro budget** under Duration discipline) — a line that runs
~6s cannot sit in a 5s tail. If it doesn't fit, resolve the runtime before delivering; don't ship a
prompt that can't lip-sync.

## Duration discipline

Default to the source clip's exact runtime. When the user changes the runtime, **recompute** any
numeric zoom timing and tell them the new mark. When a long hold lands on a static creature, add small
"living" micro-movements (a slow blink, jaw shift, steady breath) so it doesn't look frozen.

### Prepended-intro budget: intro + remaining = total

When you prepend a beat (a reveal, a telephoto hold, an establishing creature shot) to footage you must
preserve, the preserved take does not get longer — it gets *pushed back*. State the arithmetic every
time and flag what falls off:

`total runtime − intro length = surviving window for the source performance`

If the source take is longer than that surviving window, some of it cannot play. Say so explicitly and
offer the three resolutions, in order of fidelity:

1. **Extend the total** so the full source fits (intro + full source). Highest fidelity, longest clip.
2. **Start the source earlier** — sacrifice the clip's own quiet lead-in so the dialogue still lands in
   the window. Keeps total fixed, keeps the words, loses pre-roll.
3. **Accept truncation** — the first N seconds of the source won't appear. Only safe if the dropped head
   has no dialogue.

Never promise "100% lip-sync" and a prepended intro on a fixed total without doing this subtraction
first. Recompute and re-flag it on *every* change to either number.

## First / start-frame workflow

Before spending video credits, it's often worth generating the **transformed opening still** as an
image, locking the look, then animating from it. **See `references/first-frame.md`** for the full
procedure (model, settings, inputs, upload mechanics, and how to hand the still back to Seedance).

## Iterating

The user iterates fast and in small steps ("softer light," "from the right," "bigger snowier
mountains," "make the chimp huge," "a beat before the zoom," "keep the original runtime"). Change
**only the named thing** and keep the rest of the prompt stable — re-rolling the whole prompt loses
what already worked. When refining a generated still, edit the chosen result (pass it back as the
base) and fix only what's off rather than starting over.

---

## Output format

Output in **English first**, plain text — no bold, no headers, no bullets inside the prompt, not in a
code block. Easy to copy as-is. Chinese translation only if asked, after the English, same format.

A short label above each prompt (e.g. `Hook_2 · Variant 1 — Through the clouds`) is fine and helps when
you deliver several variants; the prompt body itself stays plain text.

Skeleton:

```
@source: ...
@creature: ...            (only if a texture reference is used)

Photoreal. 16:9. <N>s. <look/grade>. NON-IP — generic <X>. SFX [and source dialogue] only.

<Continuous shot, same framing as source. Preserved performance. The transformation, with physics
and plate interaction. Any timed camera move with semantic + numeric anchor. Lock-down clause: face
and identity unchanged; everything else identical to the source.>

SFX [and source dialogue] only: <specific, ordered sounds>.
```

## Voice (match the user)

Terse and kinetic; physically precise (exact materials, behaviors, scale); director-minded (lenses,
angles, moves); non-generic (no "beautiful / stunning / amazing" — texture words instead);
emotionally controlled. Don't inflate, don't soften, don't explain what things "represent."

## Seedance 2.0 input limits (reference)

Images ≤ 9; videos ≤ 3 items, total ≤ 15s; audio ≤ 3 MP3s, total ≤ 15s; total mixed inputs ≤ 12;
generation duration 4–15s. A source clip plus a texture-reference photo fits easily. If a request
needs more inputs than allowed, flag it and say what to prioritize.

## Structure patterns to internalize (style reference only — do not reproduce)

- **Add-element:** `@source (preserve all, add effect)` → `specs + NON-IP + SFX only` → `continuous
  shot, preserved performance, effect igniting/creeping with plate interaction, subject unfazed` →
  `lock-down clause` → `SFX`.
- **Environment-swap:** `@source (preserve subject + vehicle + rig + motion, replace world)` → `specs
  + grade for the new world` → `continuous shot from the same rig, new world streaming past with
  parallax, relight to match or relight-all` → `lock-down` → `SFX`.
- **Creature-on-landmark with timed zoom:** `@source` + `@creature (texture ref)` → `specs + NON-IP +
  SFX and source dialogue only` → `continuous locked shot, giant photoreal creature integrated on the
  landmark, subject delivering to camera` → `at ~T / on the line "…", smooth push-in keeping the
  landmark in frame, creature turns to camera` → `lock-down` → `SFX and dialogue`.
- **Prepended reveal intro (transform + outward move + preserved performance):** `@source (preserve
  subject + performance + lip-sync + framing, add element on landmark, prepend a telephoto intro)` +
  `@creature (texture ref)` → `specs + NON-IP + SFX and source dialogue only` → `open tight/telephoto on
  the added element in isolation for the intro beat, hard or smooth zoom-out at ~T landing on a 100%
  match of the source composition, then the preserved take plays with exact lip-sync to the quoted line
  while the added element continues behind` → `budget check (intro + remaining = total)` → `lock-down` →
  `SFX and dialogue`.
