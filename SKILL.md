---
name: tiktok-product-video
description: Create a 15-second TikTok Shop product-sales video from one or more product images and selling points. Use when a user wants to analyze a product image with a vision model, turn features into a TikTok-style conversion storyboard and natural English voiceover, and generate or prepare a 15-second reference-image video for Seedance, Kling, Sora, or another video model. Supports a user-selected aspect ratio; default to vertical 9:16.
---

# TikTok Product Video

Create an end-to-end, image-faithful 15-second product video. Default to `9:16` unless the user specifies another aspect ratio. Never infer unsupported technical, safety, performance, waterproofing, certification, price, or competitor claims.

## Required inputs

Collect:

- At least one clear product image.
- Selling points, specifications, and permitted claims.
- Optional product name, target market/language, creator profile, video model, and aspect ratio.

If a required input is absent, ask only for that input. Infer the product name from the image only when it is clear; otherwise use a neutral accurate descriptor.

## Seedance local configuration

For a Volcano Ark / Seedance render, read `ARK_API_KEY` and `ARK_BASE_URL` from the workspace file `.seedance.local.env`. The base URL must be `https://ark.cn-beijing.volces.com/api/v3` for the Ark SDK. Read `TOS_ACCESS_KEY_ID`, `TOS_SECRET_ACCESS_KEY`, `TOS_BUCKET`, `TOS_REGION`, `TOS_ENDPOINT`, `TOS_OBJECT_PREFIX`, and `TOS_URL_TTL_SECONDS` from the same file to upload local reference images to Volcano TOS. Require an IAM sub-user or temporary credential restricted to this bucket and prefix; never use the root account's unrestricted AccessKey. Never ask the user to paste secrets in chat, include them in prompts, print them, or add them to a command transcript. Stop before uploading or submitting a billable task if a required value is missing.

Before presenting a render-ready draft, collect the following runtime settings; retain them for that draft only:

- Exact model ID.
- Resolution.
- Aspect ratio.

Treat these as per-render choices; do not store them in `.seedance.local.env`.

## Workflow

1. Use a vision-capable model to inspect the supplied image. Record product category, visible materials/colors/forms, controls/ports, packaging, logos/text, likely use contexts, and visual constraints. Treat uncertain observations as uncertain.
2. Combine the visual analysis with the supplied selling points. Select the single strongest, demonstrable functional contrast; rank up to three claims by practical pain-point relief. Create a fictional generic alternative only as a visual comparison, never identify or imitate a real competitor.
3. Read [the prompt specification](references/prompt-spec.md). Generate the exact four-section production prompt and English UGC voiceover required there. Keep the pictured product identical throughout.
4. Present the complete production prompt and the model ID, resolution, ratio, duration, and audio setting. Explicitly ask the user to either confirm generation or provide edits. Do not upload images to TOS, call Seedance, create a task, or incur any generation cost at this stage.
5. If the user provides edits, apply them to the production prompt while preserving only supported claims and the original product identity. Present the complete revised prompt again and wait for an explicit confirmation. Repeat this revision loop until the user explicitly confirms generation. Do not treat silence, acknowledgement, or a request for a summary as confirmation.
6. Only after explicit confirmation, upload each local reference image to the private TOS bucket under `TOS_OBJECT_PREFIX` with a unique non-guessable object name, using `TOS_ENDPOINT` and `TOS_REGION`. Generate an HTTPS pre-signed GET URL for at least 30 minutes using `TOS_URL_TTL_SECONDS`; do not set a public bucket or object ACL. Automatically place that returned URL into `content` as `{"type":"image_url","image_url":{"url":"..."},"role":"reference_image"}`. Reuse an already-supplied HTTPS image URL without uploading it again.
7. Initialize the Ark SDK with `base_url=ARK_BASE_URL` and `api_key=ARK_API_KEY`, both read only from `.seedance.local.env`. Submit the image-to-video request through `client.content_generation.tasks.create` with the confirmed production prompt, generated `image_url` reference(s), `duration=15s`, and confirmed settings. Include `resolution` only when the chosen model supports it.
8. After task creation, tell the user the task was submitted and schedule the first `client.content_generation.tasks.get(task_id=...)` call for 1 minute later. Do not query immediately and do not block the active task with a sleep; use the available delayed wakeup or automation mechanism.
9. At the first query, treat `succeeded` and `failed` as terminal task states. On success, use only the result URL returned by the API. If the query call fails, or the task status is `failed`, promptly tell the user the task ID and current status/error without exposing credentials; schedule exactly one status re-query for 5 minutes later and tell the user it has been scheduled. Never submit a second generation task automatically.
10. At the 5-minute re-query, notify the user of the returned status. If it is still non-terminal, report that it remains in progress and do not continue background polling unless the user asks. If delayed wakeup or automation is unavailable, state that the re-query could not be scheduled and ask the user to return after 5 minutes.
11. Verify a successful returned video before delivery: approximately 15 seconds, requested framing, product identity retained, no claim contradiction, readable on-screen text, four beats present, and no accidental logos/watermarks. If the video model is unavailable, deliver the model-ready prompt and clearly say that video generation could not be run in the current environment.

## Safety and creative constraints

- Make the hook about functional contrast, never price comparison.
- Use only features supported by the image or user-provided information. Do not manufacture numeric specifications; omit a requested numeric visual anchor if no verified number is supplied.
- Depict safe, plausible use. Do not show hazardous or prohibited behavior, medical outcomes, counterfeit branding, or deceptive before/after claims.
- Keep English voiceover casual, creator-like, and free of formal ad-read language.
- Place any render-specific controls (seed, motion strength, reference adherence) outside the required prompt unless the chosen video model needs them.

## Delivery

Before a render is confirmed, provide the complete production prompt in a copyable block, confirmed draft settings, and a concise request for confirmation or edits. When a render succeeds, provide the video and the exact prompt used.
