# Project Image Replacement Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace all five low-resolution homepage project images with the exact 1456×1092 PNG originals supplied by the user.

**Architecture:** Store one canonical PNG per project under `assets/projects/` and reference it directly from both the thumbnail and full-image link in `index.html`. Remove the superseded SVG/WebP assets and the incomplete base64 reconstruction workflows so no automated process can restore stale images.

**Tech Stack:** Static HTML, PNG assets, PowerShell verification, Git

---

### Task 1: Validate and install the original PNG assets

**Files:**
- Source: `E:\下载\ruizhe_project_images_hd.zip`
- Create: `assets/projects/pace.png`
- Create: `assets/projects/masksource.png`
- Create: `assets/projects/bonevibauth.png`
- Create: `assets/projects/voice-defense.png`
- Create: `assets/projects/arc-solver.png`

- [ ] **Step 1: Extract the archive into a temporary directory**

Run PowerShell `Expand-Archive` against the exact ZIP path and a temporary directory under the repository.

- [ ] **Step 2: Verify all source files before installing them**

Run a PowerShell image check using `System.Drawing.Image.FromFile`. Expected: each mapped file reports width `1456`, height `1092`, and PNG format.

- [ ] **Step 3: Copy each file to its canonical project path**

Use the mapping from the approved design and copy bytes without resizing or recompression.

- [ ] **Step 4: Verify byte-for-byte copies**

Run `Get-FileHash -Algorithm SHA256` on each source/destination pair. Expected: every pair has identical hashes.

### Task 2: Update homepage references and remove stale assets

**Files:**
- Modify: `index.html`
- Delete: `assets/projects/pace.svg`
- Delete: `assets/projects/pace.webp`
- Delete: `assets/projects/masksource.svg`
- Delete: `assets/projects/bonevibauth.svg`
- Delete: `assets/projects/voice-defense.svg`
- Delete: `assets/projects/arc-solver.svg`
- Delete: `assets/hd/`
- Delete: `.github/workflows/build-hd-images.yml`
- Delete: `.github/workflows/build-fullhd-images.yml`

- [ ] **Step 1: Verify the old references are present**

Run `rg -o "assets/projects/(pace|masksource|bonevibauth|voice-defense|arc-solver)\.(svg|webp)" index.html`. Expected: ten matches, two per project.

- [ ] **Step 2: Replace links with canonical PNG paths**

Update both the `<a href>` and `<img src>` for each project so the extension is `.png`; do not change surrounding HTML, text, CSS, or ordering.

- [ ] **Step 3: Remove superseded image assets and reconstruction workflows**

Delete only the exact paths listed above. Keep the five PNG assets and `assets/avatar.svg`.

### Task 3: Verify and commit the completed replacement

**Files:**
- Verify: `index.html`
- Verify: `assets/projects/*.png`

- [ ] **Step 1: Check reference counts**

Run `rg -o "assets/projects/(pace|masksource|bonevibauth|voice-defense|arc-solver)\.png" index.html` and group matches. Expected: exactly two references for each PNG.

- [ ] **Step 2: Check local targets and image dimensions**

Parse every local `assets/projects/...` reference from `index.html`, confirm the target exists, and use `System.Drawing` to confirm all five project images are 1456×1092.

- [ ] **Step 3: Check for stale image references and repository state**

Run `rg -n "assets/projects/.*\.(svg|webp)|assets/hd|build-(full)?hd-images" index.html .github assets`. Expected: no stale project-image matches. Run `git diff --check`; expected: no whitespace errors.

- [ ] **Step 4: Commit the replacement**

Stage `index.html`, `assets/`, `.github/workflows/`, and this plan, then commit with message `Replace project images with HD originals`.
