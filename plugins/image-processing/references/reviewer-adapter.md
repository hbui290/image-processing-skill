# Optional structured image review

Use this only for repeated, high-stakes, or independent candidate review. A direct visual inspection remains enough for a simple edit. The script is a **review and decision aid**, not an image generator, mask maker, or proof that a picture is visually correct.

## Setup on another machine

Bring the complete plugin folder and the actual clean source, candidate, and approved references. Python 3, [Pillow](https://pillow.readthedocs.io/en/stable/installation/basic-installation.html), and [jsonschema](https://python-jsonschema.readthedocs.io/en/stable/) are needed for the script. Install them in a project-specific virtual environment only when this optional adapter is chosen. `--model` additionally requires an authenticated [Codex CLI](https://github.com/openai/codex) and an explicitly selected model that actually accepts image input on that account. It consumes account usage; get authorization for that call when not already granted. Do not paste credentials into briefs or logs.

The plugin itself installs none of these dependencies. If they are unavailable, follow [review-loop.md](review-loop.md) manually and record the limits of the review.

## Input contract

Create `brief.json` using [brief.schema.json](../skills/image-verify/references/brief.schema.json). Give each requested change and protected property a unique ID; include decoded file constraints under `file_checks`. The reviewer must inspect the real images and produce one verdict per ID using [review.schema.json](../skills/image-verify/references/review.schema.json). `pass`, `fail`, and `uncertain` require specific visible evidence. Do not manufacture a report from the prompt alone.

Example brief:

```json
{
  "name": "mug-handle",
  "intent": "Repair one mug while preserving the adjacent hand",
  "criteria": [
    {"id": "R1", "target": "A:#7 mug", "kind": "requested", "requirement": "One continuous handle attached to the mug"},
    {"id": "P1", "target": "A:#8 hand", "kind": "protected", "requirement": "Keep the original finger count and overlap"}
  ],
  "file_checks": {"width": 2048, "height": 1024, "format": "PNG", "alpha_required": false}
}
```

Example report after actually inspecting the source and candidate:

```json
{
  "criteria": [
    {"id": "R1", "status": "fail", "evidence": "The handle ends before reaching the upper rim", "suggested_fix": "Connect the upper joint only"},
    {"id": "P1", "status": "pass", "evidence": "The visible fingers and overlap match the source", "suggested_fix": ""}
  ],
  "summary": "The mug needs one local repair; the hand is preserved."
}
```

## Run a round

Use an existing visual reviewer or inspect the images yourself and save its structured verdict as `visual-report.json`. Then run the file checks and bounded decision without another model call:

```bash
python3 plugins/image-processing/skills/image-verify/scripts/review.py \
  --brief brief.json --source source.png --candidate candidate.png \
  --report visual-report.json --out review-round-0
```

For an independent Codex visual review, replace `--report visual-report.json` with `--model MODEL_ID`. Check CLI image-input support and account usage before invoking it. Set `--previous review-round-0/report.json --repairs-used 1` on the next repair round. Use a new output directory each time; the script refuses to overwrite evidence.

Read `decision.json`: `accepted_by_checks` means these specified checks passed, not that the user approved the image. `repair` provides a narrow `repair-prompt.txt`; inspect its suggested changes before using them, edit from the last accepted clean image, then recheck **all** criteria. `escalate` and `stop_*` require investigation or a user decision. `file-checks.json` records dimensions, format, transparency, and a candidate hash. The optional CLI path also writes `run.json` and local diagnostics in `.private/`; inspect those files before sharing because they can contain local paths or tool output. Neither path proves exact preserved pixels; use a separate numerical comparison outside the final acceptance mask when that is required.

This adapter is adapted from [Image Loop](https://github.com/codejunkie99/image-loop); see its [MIT attribution](../third_party/image-loop/NOTICE.md).
