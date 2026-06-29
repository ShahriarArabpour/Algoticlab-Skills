# First Frame Workflow

Use this reference when the user wants to generate a transformed opening still before spending video credits in Seedance 2.0.

## Goal

Create a still image that locks the transformed look, scale, lighting, and integration before asking Seedance to animate the full video-to-video transformation.

## When To Use

- The transformation is expensive or likely to drift.
- A creature, fire, invisibility effect, environment swap, or hard-surface element must look photoreal.
- The user wants to approve the first visual frame before animation.
- The output kept looking CG, pasted-in, off-scale, or poorly relit.

## Workflow

1. Describe the source frame:
   - subject identity, wardrobe, pose, camera angle, lens feel, lighting direction, environment, aspect ratio.
2. Define the transformation:
   - added element, environment swap, lighting/grade change, scale, physics, contact shadows, atmospheric integration.
3. Write a still-image prompt in English:
   - Preserve identity and source framing.
   - Transform only the named element.
   - Match key direction, grain, depth of field, haze, and edge behavior.
4. Ask the user to select or approve the still.
5. Use the approved still as the transformed start-frame input for Seedance, while still declaring the original source clip as the motion/performance source.

## Still Prompt Pattern

```text
Photoreal transformed opening frame based on the source clip. Preserve the subject's face, identity, wardrobe, pose, camera angle, framing, lens character and lighting direction exactly. Add only <transformation>. The added element shares the same key direction, shadow density, atmospheric haze, film grain and depth of field as the source plate. No CG look, no plastic texture, no halos, no hard cut-out edges.
```

## Animation Handoff Pattern

```text
@source: Original source clip — preserve identity, performance, camera motion and framing exactly.
@start_frame: Approved transformed still — use only as the opening visual lock for the added element, lighting integration and scale.
```

## Failure Checks

- Do not let the still become a new scene if the task is video-to-video transformation.
- Do not change the face, wardrobe, camera angle, or source performance unless the user explicitly asks.
- Do not skip contact shadows, environmental bounce, haze, grain, and edge integration.
- Do not animate from an unapproved still when the user asked to approve the look first.
