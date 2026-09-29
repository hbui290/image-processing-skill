---
name: image-processing
description: Create or repair raster images when source fidelity, identity, geometry, crop, resolution, transparency, or delivery quality needs deliberate control. Use for supplied or AI-generated art, photos, products, and web assets; for critique-only requests, diagnose without editing.
---

# Image processing

Produce an image that is accurate to the user's source and usable at its intended display size. A larger pixel count is not proof of more real detail. A convincing AI edit is not proof that protected subjects stayed the same.

## Establish the image contract

- Identify the image's job and delivery surface: hero, portrait, product image, thumbnail, illustration, background, print, game art, or another target. Record required dimensions, aspect ratio, format, transparency, file-size budget, and where it will be viewed.
- Identify the source of truth for each protected element: original image, approved character sheet, product photo, logo/vector, diagram data, or user-supplied reference. Record which items may change and which must remain fixed.
- Keep an untouched source. Save candidates as new files until accepted. Do not silently replace a master or claim that an AI reconstruction is a lossless restoration.
- For a new image with no source to preserve, define subject, composition, style, and approved references; generate candidates using the selected tool's instructions and judge them against the image contract. Preservation masks apply when editing an existing image.
- When only an assessment is requested, inspect and report; edit only if the user requests a change.

## Check tool readiness on a new machine

Count capabilities before products: **0** image-editing tools for critique, **1** for a deterministic crop/mask/export, **2** for generating missing detail and compositing accepted pixels, with **+1** for useful automatic object selection or a separate super-resolution pass. A viewer is needed for review but is outside that count. One application may cover several capabilities.

Read [tool-readiness.md](references/tool-readiness.md) when a new agent takes over or the machine is unfamiliar or reinstalled. Check the source, references, format support, generation access when needed, and native/delivery-size viewing. Use an available equivalent when it provides the same control; otherwise follow the authorized setup process. Copying the skill does not install tools or transfer source art. Keep segmentation, restoration, and web build tools conditional.

## Diagnose at two scales

Inspect the source at native pixels **and** the rendered result at the target size. For a web image, check CSS crop/fit, overlays, blur filters, opacity, device pixel ratio, and responsive positions before editing pixels. For print, inspect the physical size and effective resolution. Use close crops to locate defects, then review the entire composition.

| Finding | First useful action |
| --- | --- |
| Wrong crop, contrast, or layout | Correct framing or presentation; preserve the image pixels if they are sound. |
| Compression blocks, noise, mild softness | Try measured denoise, deblock, or sharpening; compare fine details before accepting. |
| Too few pixels but recognizable detail | Try super-resolution suited to the image medium; compare at final size and reject oversmoothing or invented facial or product details. |
| Missing eyes, broken geometry, fused objects, extra limbs, inconsistent texture | Use a small local edit, manual retouch, or compositing with an approved reference. Upscaling alone cannot solve missing semantics. |
| Damaged alpha edge or unwanted background | Repair the matte or background, then inspect the edge against light and dark surfaces. |
| Composition fundamentally wrong | Recompose or regenerate only when that larger change is within the user's request. |

Choose the least disruptive operation that solves the observed defect. Use specialist generation or editing tools according to their own instructions when pixels must be generated. Use deterministic image tools for cropping, masking, color correction, resizing, and export when those operations are sufficient.

For connected structures or crowded scenes, map supports, openings, overlaps, and repeated object instances before editing. Check structure and proposed masks against source pixels. Do not infer a proprietary editor's internal algorithm from an open-source example.

## Control generated edits

- Supply the relevant crop and approved references, identifying what each controls. Specify the exact change and protected subjects, count, pose, product markings, typography, perspective, and layout. Composite approved logos or exact text from their source art.
- Prefer local repair when the composition is sound. Branch from the approved source rather than feeding each generated output into the next edit. A prompt alone cannot protect pixels outside the requested change.
- Use a crop with context for lighting and geometry, then accept only reviewed pixels through a final mask. Keep the model's input mask separate from the final acceptance mask; check polarity, full object coverage, neighboring subjects, alignment, feathered edges, and seams.
- Treat novel details as proposed art, not recovered facts. If reference material cannot establish an identity-critical feature, report the uncertainty or request a better reference instead of inventing a canonical answer.

Read [repair-and-composite.md](references/repair-and-composite.md) when a task needs object IDs, tiny-object tiles, prompt structure, crop geometry, feathered masks, or a command example. The reference is a method, not a fixed set of coordinates or model settings.

## Accept or reject the result

1. **Identity and meaning:** Compare protected faces, products, animals, hands, clothing, marks, logos, labels, and object count against approved sources. Reject identity drift, duplicated items, misspelled text, and changed meaning.
2. **Local craft:** At 100% scale, check geometry, perspective, repeated textures, halos, seams, smeared edges, and inconsistent grain or line weight. For connected objects, follow supports, joints, occlusion, and usable paths across the crop boundary.
3. **Whole composition:** Inspect the final crop at every relevant target size. Confirm focal point, negative space, legibility, and the relationship between image and overlaid content.
4. **File delivery:** Verify dimensions, orientation, color profile when relevant, transparency, format, decoding, file size, and actual references in the destination. Preserve an editable master when future changes are likely.
5. **Evidence:** State the source, operations, generated regions, rejected candidates that affected the decision, what was visually checked, and what remains unverified. Separate tools actually run from tools merely installed or proposed, and verbatim prompts from reconstructed examples. Distinguish a local preview from a deployed result. Do not present a successful export or build as proof of visual quality.

When a protected region must remain exact, compare the source and uncompressed composite outside the nonzero final acceptance mask. Investigate every changed pixel there before lossy export; then inspect the exported image visually because compression may change even protected pixels. Check seams both numerically and at 100% view.

If a candidate repeatedly changes protected content, stop rerunning the same full-image prompt. Narrow the crop, strengthen the reference, use a manual edit, or keep the prior version. The correct outcome can be to reject an AI result.
