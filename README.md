# Image Processing Skill

**Turn imperfect image assets into reviewable, production-ready work—without losing the details that matter.**

![Version](https://img.shields.io/badge/version-v1.0.0-0B6E75) ![Format](https://img.shields.io/badge/format-Agent%20Skill-2D4059)

AI-generated images can look convincing at a glance while hiding changed faces, bent structures, duplicated objects, or soft detail. This skill gives an agent a practical way to diagnose those defects, repair only the necessary pixels, and check the result at its real display size. It also covers creating a new image from an approved brief and references.

## What it helps with

- **Find the real cause:** inspect source pixels and the delivered crop before changing an asset. A CSS blur or poor crop needs a different fix from missing facial detail.
- **Protect the source:** record approved references, fixed subjects, and permitted changes; keep the original untouched.
- **Repair locally:** use a source crop, a proposed replacement, and a reviewed acceptance mask to composite only approved pixels.
- **Review honestly:** inspect identity, object count, geometry, seams, and final rendering; distinguish tools actually run from tools merely available.
- **Move machines safely:** use the readiness guide to check capabilities, source files, model access, and output formats before continuing a job.

## Start here

1. Copy this entire repository into your agent's configured skills directory as `image-processing/`, then start a new session and confirm the skill appears in its available-skill list. For a typical Codex POSIX setup, the destination is `${CODEX_HOME:-$HOME/.codex}/skills/image-processing/`.
2. Read [SKILL.md](SKILL.md). It is the short decision workflow; the two references are opened only when relevant.
3. On a new machine or with a new agent, follow [tool readiness and setup](references/tool-readiness.md). Bring the original image, approved references, accepted candidates, and masks for any job you want to continue.
4. For a local replacement, follow [repair and compositing](references/repair-and-composite.md). Review the uncompressed composite before exporting the delivery format.

The skill is a workflow, **not** an image model or an installer. One application can cover several capabilities. An assessment needs no editing tool; a deterministic crop or composite needs one; generating missing detail and compositing the accepted result needs two capabilities. Segmentation and super-resolution are optional additions.

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

## Release

**v1.0.0** — initial public skill release. It includes the core workflow, a portable tool-readiness checklist with source links, and local repair recipes for POSIX shells and PowerShell. Use the Git tag `v1.0.0` to obtain this exact version.
