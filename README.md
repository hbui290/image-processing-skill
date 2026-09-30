# Image Processing Plugin + Skill

**Turn imperfect image assets into reviewable, production-ready work—without losing the details that matter.**

![Version](https://img.shields.io/badge/version-v1.4.1-0B6E75) ![Format](https://img.shields.io/badge/format-Codex%20Plugin%20%2B%20Agent%20Skill-2D4059)

AI-generated images can look convincing at a glance while hiding changed faces, bent structures, duplicated objects, or soft detail. This repository now provides a modular Codex plugin for focused image tasks and retains the original standalone skill for existing installations. Neither package includes an image model.

## Choose the entry point

| Use | Location | Responsibility |
| --- | --- | --- |
| Inspect or identify a target | [image-inspect](plugins/image-processing/skills/image-inspect/SKILL.md) | Diagnose the source and map ambiguous objects without editing |
| Create new artwork | [image-create](plugins/image-processing/skills/image-create/SKILL.md) | Generate from a brief and role-limited references |
| Repair an accepted image | [image-repair](plugins/image-processing/skills/image-repair/SKILL.md) | Produce a local candidate and composite reviewed pixels |
| Check a candidate | [image-verify](plugins/image-processing/skills/image-verify/SKILL.md) | Review required changes, protected details, and delivery quality without editing |

The [plugin manifest](plugins/image-processing/.codex-plugin/plugin.json) packages these four skills. Its detailed references live inside the plugin, so the package can travel on its own. An agent may use the relevant skills in sequence for a complete job; independent review is optional when the task justifies it. The top-level [SKILL.md](SKILL.md) remains the backward-compatible standalone workflow and is no longer where new specialist instructions accumulate. Choose one installation mode per agent to avoid duplicate routing between the legacy skill and the plugin.

## Install the plugin in Codex

After this release is available on GitHub, register the repository marketplace and install the package:

```bash
codex plugin marketplace add hbui290/image-processing-skill --ref v1.4.1
codex plugin add image-processing@image-processing
```

Start a new Codex chat after installation so its four skills are discovered. The [marketplace entry](.agents/plugins/marketplace.json) points to the self-contained plugin folder. Installing the package does not install an image model, ImageMagick, Real-ESRGAN, or Grounded SAM 2; check the task-specific capabilities in [tool readiness](plugins/image-processing/references/tool-readiness.md). No marketplace or account configuration changes occur merely by cloning this repository.

The [optional candidate audit](plugins/image-processing/references/candidate-audit.md) is built for local image repairs: it connects stable object IDs, source-pixel boxes, a final acceptance mask, and a real visual review. It measures exact decoded-pixel changes outside the mask and flags a claimed change when its target contains no changed pixels. It needs only Python and Pillow when selected; image generation and visual judgment remain with the host.

## What it helps with

- **Find the real cause:** inspect source pixels and the delivered crop before changing an asset. A CSS blur or poor crop needs a different fix from missing facial detail.
- **Protect the source:** record approved references, fixed subjects, and permitted changes; keep the original untouched.
- **Repair locally:** use a source crop, a proposed replacement, and a reviewed acceptance mask to composite only approved pixels.
- **Pinpoint crowded scenes:** identify repeated objects on a separate numbered review copy while keeping the clean source as the edit input.
- **Review honestly:** check every requested and protected criterion, stop unproductive repair loops, and distinguish tools actually run from tools merely available.
- **Move machines safely:** use the readiness guide to check capabilities, source files, model access, and output formats before continuing a job.

## Standalone skill setup

1. For the existing standalone mode, copy this repository into your agent's configured skills directory as `image-processing/`, then start a new session and confirm the skill appears in its available-skill list. For a typical Codex POSIX setup, the destination is `${CODEX_HOME:-$HOME/.codex}/skills/image-processing/`.
2. Read [SKILL.md](SKILL.md). It is the short decision workflow; supporting references are opened only when relevant.
3. On a new machine or with a new agent, follow [tool readiness and setup](references/tool-readiness.md). Bring the original image, approved references, accepted candidates, and masks for any job you want to continue.
4. For a local replacement, follow [repair and compositing](references/repair-and-composite.md). Review the uncompressed composite before exporting the delivery format.
5. For multiple candidates or repair rounds, use the optional [bounded review and handoff](references/review-loop.md) guide. It requires no extra software.

The skill is a workflow, **not** an image model or an installer. One application can cover several capabilities. An assessment needs no editing tool; a deterministic crop or composite needs one; generating missing detail and compositing the accepted result needs two capabilities. Segmentation and super-resolution are optional additions.

The standalone setup above remains available when no plugin marketplace is configured. Do not install both entry points in the same agent unless you intentionally want overlapping skill routing.

## Tool map

| Tool | Role | When to use it | Source |
| --- | --- | --- | --- |
| ChatGPT ImageGen | Reference-guided image generation or editing | Proposed pixels for missing semantic detail; access must be checked on each account | [OpenAI image documentation](https://developers.openai.com/api/docs/guides/tools-image-generation) · [ChatGPT Images help](https://help.openai.com/en/articles/11084440-images-in-chatgpt). The hosted model has no public source repository. |
| ImageMagick | Exact crops, masks, composites, comparisons, export | Documented command-line example for controlled pixel work; a capable editor can replace it | [GitHub](https://github.com/ImageMagick/ImageMagick) · [installation](https://imagemagick.org/download/) |
| Real-ESRGAN | Super-resolution | Only when recognizable detail needs more display pixels | [GitHub](https://github.com/xinntao/Real-ESRGAN) |
| Grounded SAM 2 | Grounded object selection and mask proposals | Optional for crowded scenes with many objects | [GitHub](https://github.com/IDEA-Research/Grounded-SAM-2) · [setup](https://github.com/IDEA-Research/Grounded-SAM-2/blob/main/INSTALL.md) |
| SAM 2 | Segmentation | Optional component for object masks | [GitHub](https://github.com/facebookresearch/sam2) |
| libwebp / `cwebp` | WebP encoding | Optional if the chosen editor cannot export the needed WebP | [GitHub](https://github.com/webmproject/libwebp) |

Other conditional options documented in the references: [LaMa](https://github.com/advimman/lama) for inpainting, [CodeFormer](https://github.com/sczhou/CodeFormer) for face restoration, and [Diffusers](https://github.com/huggingface/diffusers) for model-based editing. [Vite](https://github.com/vitejs/vite) can be part of a website delivery check; it is not an image-editing dependency. macOS `sips` is a system utility, not a separate repository.

**Provenance:** In the Jadebound hero repair that informed this workflow, ImageGen produced replacement crop candidates, ImageMagick handled crops, masks, composites, and export, and Real-ESRGAN was used for an early resolution pass. `cwebp` encoded early WebP candidates; `sips` measured dimensions. Grounded SAM 2 was researched later but was **not** used to produce that hero. Those tools are examples, not an install-all checklist.

## Related work

The plugin's methods draw on [Image Loop](https://github.com/codejunkie99/image-loop/blob/main/skills/image-loop/SKILL.md) for bounded checks, [Image Edit Map](https://github.com/codejunkie99/image-loop/blob/main/skills/image-edit-map/SKILL.md) for addressable objects, [Image Reconstruction](https://github.com/codejunkie99/image-loop/blob/main/skills/image-reconstruction/SKILL.md) for separating source evidence from edit decisions, [Visual Design Kit](https://github.com/newmindsgroup/visual-design-kit/blob/main/plugins/visual-design-studio/library/templates/media-quality-rubric.md) for reject/hold decisions, and [BuilderIO Logo Composite](https://github.com/BuilderIO/agent-native/blob/main/templates/assets/.agents/skills/logo-composite/SKILL.md) for exact brand overlays. These are credited research sources, not runtime dependencies. The candidate-audit script and its source-pixel mask checks are this package's implementation.

## Release

**v1.4.1** — adds a same-size super-resolution comparison for large but soft sources and nested masks that preserve exact details inside a repaired object. The readiness guide now distinguishes installed tools from models that were actually verified and run.

**v1.4.0** — replaces the copied Image Loop reviewer with a purpose-built candidate audit for local repair: object IDs, source-pixel boxes, decoded-pixel checks outside the final mask, full review coverage, and bounded decisions. Requires only optional Pillow; no nested Codex run or model use. Image Loop remains credited as research inspiration.

**v1.3.0** — adapted Image Loop's reviewer/controller under its MIT license. This implementation remains available at the historical `v1.3.0` tag and was replaced in v1.4.0.

**v1.2.0** — adds a modular Codex plugin package with four focused skills and self-contained references. The v1.1 standalone skill stays available for existing agents. No model, external account, or heavyweight segmentation dependency was added.

**v1.1.0** — adds optional numbered object maps, criterion-by-criterion review, bounded repair rounds, and a portable handoff record without new runtime dependencies. Use the Git tag `v1.1.0` to obtain this exact version.

**v1.0.0** — initial public skill release with the core workflow, portable tool-readiness checklist, source links, and local repair recipes for POSIX shells and PowerShell.
