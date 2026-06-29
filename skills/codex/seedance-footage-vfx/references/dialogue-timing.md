# Dialogue Timing

Use this reference when a Seedance footage-transformation prompt must sync a camera move, creature reveal, lip-sync moment, or VFX payoff to spoken dialogue or a specific timecode in the source clip.

## Goal

Anchor timing two ways:

1. Semantic cue: the exact spoken words or visible event.
2. Numeric cue: approximate seconds from the start of the source clip.

Never invent precise timing. If the clip cannot be inspected and the user did not provide a timecode, ask for the timecode or use a clearly marked approximate cue.

## Workflow

1. Identify the source runtime, fps, aspect ratio, and whether audio/dialogue must survive.
2. Locate the line or event:
   - If the user provides a timecode, convert it to seconds.
   - If the clip is accessible locally, inspect the waveform/transcript or extract frames around the moment.
   - If neither is available, ask for the approximate time or exact spoken line.
3. Write both anchors in the action:
   - `On the line "<exact words>," the camera begins a smooth push-in...`
   - `At about 3.2 seconds, the push-in starts...`
4. Set the audio spec correctly:
   - `SFX only` when no source dialogue is needed.
   - `SFX and source dialogue only` when the original spoken line must remain.
5. Check the remaining runtime after any prepended intro or reveal.

## Timecode Conversion

- `00:00:03.200` = about `3.2 seconds`
- `00:00:06:12` at 24fps = `6.5 seconds`
- `00:00:06:15` at 30fps = `6.5 seconds`

When fps is unknown, avoid frame-level precision and use approximate seconds.

## Prompt Pattern

```text
On the line "exact quoted source dialogue," at about 3.2 seconds, the camera snaps into a hard crash zoom toward the added element, while the source dialogue remains perfectly intact.
```

## Failure Checks

- Do not promise exact lip-sync if the quoted line cannot fit inside the surviving source-performance window.
- Do not use `SFX only` when source dialogue must survive.
- Do not create a prepended intro without subtracting it from the total runtime.
- Do not move the timing cue when the user only asked to change lighting, creature, or environment.
