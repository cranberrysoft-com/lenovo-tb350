# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Community research and documentation for running an open-source-oriented Android system on the Lenovo Tab P11 (2nd Gen) — `TB350FU` (Wi-Fi) and `TB350XU` (LTE), both MediaTek Helio G99.

**This repository contains no code.** There is no build, no test suite, no lint step, and no package manifest — only Markdown under `docs/`, a `README.md`, and an MIT `LICENSE`. Every task here is a documentation task. Do not add a build system, CI, or tooling unless explicitly asked.

Key docs:
- `docs/upgrade-plan.md` — the spine of the project: objective, verified baseline, staged plan (0–5), decision gates, primary references
- `docs/device-and-image-matrix.md` — in-scope hardware, explicitly incompatible models, candidate GSI classes, data still to be collected from a real device
- `docs/recovery-readiness.md` — pre-unlock checklist, in `- [ ]` task-list form

The current direction is a vanilla LineageOS 22.2 / Android 15 GSI, with a LineageOS 23.2 / Android 16 GSI as an experimental second stage. A GSI replaces the Android userspace only — bootloader, kernel, vendor HALs, modem/Wi-Fi firmware and calibration data stay Lenovo's. Never describe the outcome as a device-native ROM or a fully Lenovo-free stack.

## Decision Tracking

**Every research or scope decision must land in the docs, in the same change that makes it.** There is no separate ADR file — the three docs *are* the record:

1. A change to what gets installed, in what order, or under what preconditions → `docs/upgrade-plan.md` (stages, and the **Decision gates** list if the bar for a permanent install moves)
2. A change to which models or images are in or out of scope → `docs/device-and-image-matrix.md`
3. A change to what must be true before touching a partition → `docs/recovery-readiness.md`
4. A change visible from the outside (direction, safety posture, doc set) → `README.md`

If a new finding contradicts something already written, **edit the existing statement** rather than appending a contradictory one. The docs must never hold two answers to the same question. When a stage's status changes, update the `Status:` line at the top of `docs/upgrade-plan.md` with an absolute date.

## AI Development Workflow

This project is written primarily by AI coding agents. Mariusz acts as reviewer and decision-maker.

**Commit identity:**

- **Nex** — `nex@cranberrysoft.com` — Claude Code agent identity. Claude Code must always commit as Nex.
- **Jack** — `jack@cranberrysoft.com` — Codex agent identity.
- **Mariusz Dubielecki** — `dev@mariusz.au` — commits human-authored changes himself.

The repo has no local `user.name` / `user.email` set. Set them in the worktree before your first commit:

```bash
git config user.name  "Nex"
git config user.email "nex@cranberrysoft.com"
```

**What this means for you as an agent working on this repo:**

- **Always work on a new branch in a new worktree.** Multiple agents work on this repo in parallel; isolating each task keeps them from clobbering each other. `git worktree add ../tb350-<short-task-slug> -b <branch-name> origin/main`, then work there. Branch names: lower-kebab-case with a scoped prefix — `docs/` for documentation changes (the common case here), `chore/` for repository housekeeping, `research/` for findings not yet ready to change the plan. Never commit directly to `main`.
- **Finish every task with a PR — automatically, without waiting for confirmation.** When the work is complete, commit, push, and open a PR against `main` with `gh pr create`, with a body that summarises the change and says what was verified and how. Do not pause to ask whether to open the PR; it is pre-authorised. Do not merge it yourself — Mariusz reviews and merges. Do not delete the worktree until the PR is merged.
- **Read all three docs before editing any one of them.** They cross-reference each other and repeat constraints deliberately. A change to the staged plan usually needs a matching change to the matrix or the checklist.
- **Verify device facts against primary sources.** Every hardware claim, lifecycle date, GSI requirement or partition behaviour must trace to an official or clearly-attributed source — Lenovo PSREF/support, `source.android.com`, `developer.android.com`, or a named community project. Do not write specifications, build numbers, VNDK versions or partition layouts from memory. If a fact cannot be sourced, write that it is unverified rather than writing it as fact. Add the source to the **Primary references** list in `docs/upgrade-plan.md`.
- **Never transplant data from another device.** A similar brand name or a shared MediaTek SoC does not make boot, LK, preloader, recovery, vendor or modem images interchangeable. TB-J616F/X and TB336FU/ZU are documented as incompatible for exactly this reason. Do not import procedures, checksums or patched boot images from other models into TB350 instructions.
- **Implement the exact scope asked.** No extra sections, no unrequested restructuring, no speculative future stages.
- **Surface ambiguity, don't resolve it silently.** If a question isn't answered by the existing docs or a citable source, say so and ask — do not fill the gap with a plausible-sounding answer. In this domain a confident wrong instruction bricks a tablet.
- **Save plan-mode plans into `docs/plans/`.** Whenever a plan-mode session produces a plan, also write it to `docs/plans/<slug>.md` so the approach can be reviewed via PR. Slug is lower-kebab-case matching the intended branch name without its type prefix. Ship the plan as a standalone `docs/plan-<slug>` PR first, then open the implementation PR referencing the merged plan path — except for locally-run ultraplan sessions, which Mariusz reviews in-session and which skip the separate docs PR. Delete a per-task plan in the same commit set as the change it drove; keep research plans that will never ship a change, and note their non-shipping status in the file.
- **Check PR review comments after every push and address them.** Fetch with `gh api repos/cranberrysoft-com/lenovo-tb350/pulls/<n>/comments`, `.../issues/<n>/comments` and `.../pulls/<n>/reviews`. Reply on the finding's own thread, react on the "Useful?" prompt, and resolve the thread — a fix announced only in a top-level comment gets re-raised verbatim next cycle. Treat P1 as blocking, P2 as strongly-preferred for the change under review, P3 as opt-in. "Valid, out of scope, filed as #N" is a legitimate resolution. If Codex has left no 👀, no 👍 and no review a few minutes after the push, comment `@codex review` to request one.
- **Wait for a review in flight to land before calling a PR done.** The 👀 eye means the review is still running. Check reactions with `gh api repos/cranberrysoft-com/lenovo-tb350/issues/<n>/reactions --jq '.[] | "\(.user.login): \(.content)"'`.

## Documentation Conventions

The existing docs have a deliberate voice. Match it — new text should be indistinguishable from what is already there.

- **Neutral, factual, unexcited.** No marketing language, no "easy"/"simply"/"just", no emoji, no exclamation marks. Sentences state what is known and what is not.
- **Never overstate certainty.** The docs repeatedly qualify: "No maintained, device-native LineageOS build for the TB350 has been verified"; "Passing this checklist makes experimentation more recoverable, not risk-free". Preserve those hedges; do not "tighten" them into claims.
- **British/Commonwealth spelling** (`synchronised`, `organisation`), consistent with the existing text.
- **Absolute dates**, written out — `16 August 2026`, not "today" or "last week".
- **Backticks** for model numbers used as identifiers (`TB350FU`), modes (`fastbootd`), filenames and commands.
- **Tables** for comparisons across models or image classes; `- [ ]` task lists only in `docs/recovery-readiness.md`; plain bullets elsewhere.
- **Relative links** between docs (`recovery-readiness.md`, `docs/upgrade-plan.md` from the README).
- **Structure to reuse:** a claim, then the constraint that qualifies it. Sections close with the limitation rather than a summary.

## Safety and Redistribution Rules

These are hard constraints on the content, not style preferences:

- **Do not commit proprietary material.** No Lenovo firmware, stock packages, boot/recovery/vendor images, or extracted blobs. Link to the official source and record filename, build identifier, size and SHA-256 instead.
- **Do not commit device-identifying data.** Serial numbers, IMEIs and full account identifiers stay out of the repository, including in example command output — the recovery checklist calls this out explicitly.
- **Do not write an installation guide as if it were tested.** `docs/upgrade-plan.md` is explicitly "not yet an installation guide". Exact flashing commands belong in a separate, reviewed installation document created only after the decision gates are met — do not add them to the plan.
- **Do not use unversioned "latest" download links** in anything meant to be reproducible; pin a release and record its hash.
- **Preserve the warnings.** Bootloader unlocking factory-resets the device; never relock with a modified or unsigned system installed; stop after any unexpected error. If you rewrite a section containing one of these, the warning survives the rewrite.
