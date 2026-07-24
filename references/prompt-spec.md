# 15-second TikTok Shop prompt specification

Use this specification after visual analysis. Replace every placeholder with image-supported or user-supplied facts. The final output contains only the production prompt below—no prefatory explanation, labels outside the template, or untranslated Chinese. Preserve the product's exact colors, silhouette, proportions, placement of details, and visible branding from the reference image in every shot.

## Prewriting rules

1. Identify the category, appearance, and 1 strongest demonstrable contrast.
2. Select no more than 3 claims. Rank: scenario pain-point relief, distinctive hardware/function, then value close. A claim needs image evidence or user-provided evidence.
3. Bind each verified number to a concrete visual reference. Do not invent numbers. If none are verified, use a non-numeric visual comparison instead.
4. Use a generic ordinary alternative for comparison. Never name a competitor or make an unverifiable absolute claim.
5. Total voiceover should sound like one creator speaking naturally in about 15 seconds. Keep each required word count.

## Required output template

```text
Ad of the {{product_name}} in the picture, keep image of the {{product_name}} same in the whole video. Aspect ratio: {{aspect_ratio}}. Use the supplied product image as the identity reference; preserve its exact appearance, colors, materials, logos, and proportions in every shot.

OPENING (0-3s): [{{Choose one functional-contrast hook; never mention price. A: show an ordinary generic product failing at {{verified_pain_point}}, then at 0.5s smash-cut to this product smoothly handling the same task. B: immediately show this product's verified signature function that a generic alternative cannot perform. Fast rhythm, cuts at most 0.5s each, tight product framing, no unsupported result.}}]
VOICEOVER: "{{10-15 English words. Start with surprise or a counterintuitive observation; state only the functional difference.}}"

PRODUCT DEMO (3-10s): [Fit this entire beat into 7 seconds: macro shot of verified core hardware -> pain-point text plus a verified number as a large on-screen graphic (or non-numeric visual anchor) -> split-screen generic limitation versus this product -> macro shot of second verified feature -> pain-point/fix caption -> matched-scene comparison -> rapid montage of up to four plausible contexts. Cut or layer shots at 0.5-1s; during the final 2 seconds, overlay/transition the montage into a 360-degree product orbit with a highlight sweep. Product shape remains unchanged; captions state pain point -> product function -> practical convenience.]
VOICEOVER: "{{25-35 English words. Conventional limitation -> this product's corresponding improvement -> inconvenience removed. Avoid empty adjectives such as 'very' and 'great'. Increase pace sentence by sentence; an optional price anchor may appear only at the end and only if user supplied it.}}"

PERFORMANCE DEMO (11-13s): [Vertical center-line split screen; same location and lighting, no filter. Left: ordinary generic product's verified limitation. Right: this product's image-supported improvement. At 12s, freeze the right side; a creator enters with a small, believable 'now I get it' expression.]
VOICEOVER: "{{8-12 English words in short phrases; describe only the visible functional difference.}}"

CLOSING (13-15s): [Creator holds the product and brings it toward the lens with a calm, certain recommendation expression. Final freeze: charcoal-gray gradient background; product centered, rotating slowly; hard side light traces its silhouette. A generic alternative's fading ghost image may remain, but no price emphasis.]
VOICEOVER: "{{10-15 English words. Use a relaxed rhetorical question or direct purchase invitation; no hard-sell advertising tone.}}"

Style: UGC handheld with purpose — slight shake for authenticity but framing stays tight on product. Music cuts: each scene transition syncs to a beat drop. Natural mobile-camera texture, creator-made pacing, readable English captions inside safe margins. No product redesign, no extra brand logo, no false claims, no distorted hands or product geometry.
```

## Render handoff

Pass the reference image(s), the complete template output, 15-second duration, and aspect ratio to the video model. Prefer image-reference / identity-preservation mode. Use provider-specific settings only when documented by that provider. After rendering, check the product against the source image shot by shot and regenerate if its identity changes materially.
