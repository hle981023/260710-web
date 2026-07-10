# Red Introduction Button Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Change the `자기소개로...` button in `index.html` to a red background with white text, then publish the change to GitHub and the live FTP site.

**Architecture:** Keep the existing single-file static-site structure and change only the two color declarations in the `.next-link` CSS rule. Validate the exact CSS values and all local HTML references before publishing the commit to `main`, then upload only `index.html` to the FTP web root and verify the served HTML.

**Tech Stack:** HTML, CSS, Python standard library validation, Git, GitHub, FTP

## Global Constraints

- Use `#e60012` for the `.next-link` background.
- Use `white` for the `.next-link` text.
- Preserve the existing black border, spacing, typography, position, visibility timing, destination, and click behavior.
- Do not change buttons or links on other pages.

---

### Task 1: Change and validate the introduction button colors

**Files:**
- Modify: `index.html:45-46`
- Test: inline Python assertions against `index.html`

**Interfaces:**
- Consumes: the existing `.next-link` CSS rule in `index.html`
- Produces: `.next-link` with `color: white` and `background: #e60012`

- [ ] **Step 1: Run the failing color assertion**

```bash
python3 -c 'from pathlib import Path; s=Path("index.html").read_text(); block=s.split(".next-link {",1)[1].split("}",1)[0]; assert "color: white;" in block and "background: #e60012;" in block'
```

Expected: FAIL with `AssertionError` because the current rule uses black text and a white background.

- [ ] **Step 2: Apply the minimal CSS change**

```css
.next-link {
  color: white;
  background: #e60012;
}
```

Change only the existing `color` and `background` declarations; retain every other declaration.

- [ ] **Step 3: Run the color assertion again**

Run the Step 1 command again.

Expected: PASS with exit code 0.

- [ ] **Step 4: Run the static-site validator**

```bash
python3 ../validate_upload.py .
```

Expected: `MISSING_LOCAL_REFS 0`, `FILES_OVER_95_MIB 0`, exit code 0.

- [ ] **Step 5: Review and commit the implementation**

```bash
git diff --check
git diff -- index.html
git add index.html docs/superpowers/plans/2026-07-10-red-introduction-button.md
git commit -m "Make introduction button red"
```

Expected: the code diff changes only the two approved CSS declarations.

### Task 2: Publish and verify GitHub and FTP

**Files:**
- Publish: Git commit on `main`
- Upload: `index.html` to `/html/my/index.html`

**Interfaces:**
- Consumes: the verified committed `index.html`
- Produces: matching GitHub `main` and live FTP copies

- [ ] **Step 1: Push `main` to GitHub**

```bash
git push -u origin main
```

Expected: `main -> main` with exit code 0.

- [ ] **Step 2: Confirm the remote commit**

```bash
git ls-remote origin refs/heads/main
git rev-parse HEAD
```

Expected: both commands output the same full commit SHA.

- [ ] **Step 3: Inspect the FTP root without changing it**

Connect with the supplied FTP credentials and confirm that the existing `/html/my` document directory remains available. Do not create, rename, or delete directories.

- [ ] **Step 4: Upload only `index.html`**

Use passive FTP and binary transfer mode to replace `/html/my/index.html` with the committed local `index.html`.

Expected: FTP returns a successful transfer response and the remote byte count matches the local file.

- [ ] **Step 5: Verify the live deployment**

Fetch `http://yongbong.dothome.co.kr/my/index.html` with cache bypass and assert that the `.next-link` CSS contains `color: white` and `background: #e60012`.

Expected: HTTP request succeeds and both declarations are present in the served HTML.
