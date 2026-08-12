# sam-michaels.github.io

Personal portfolio site. Single `index.html`, no framework, no build step.

**Live at:** https://sam-michaels.github.io

---

## Deploying to GitHub Pages

### 1. Create the repository

The repo name has to be **exactly** `sam-michaels.github.io` — your GitHub username, then `.github.io`. That's what makes it a user site served at the root domain. Any other name gets you a subpath URL instead.

- Go to https://github.com/new
- Repository name: `sam-michaels.github.io`
- **Public** (GitHub Pages requires this on free accounts)
- Don't add a README, .gitignore, or licence — this folder already has what it needs

### 2. Push the files

From inside this folder:

```bash
git init
git add .
git commit -m "Portfolio site"
git branch -M main
git remote add origin https://github.com/sam-michaels/sam-michaels.github.io.git
git push -u origin main
```

### 3. Turn on Pages

Repo → **Settings** → **Pages** → under "Build and deployment", set Source to **Deploy from a branch**, branch `main`, folder `/ (root)` → Save.

First build takes 1–2 minutes. After that, every push to `main` redeploys automatically.

### 4. Check it

Visit https://sam-michaels.github.io. If you get a 404, wait another minute — the first deploy is the slow one.

---

## What's in here

| File | Purpose |
|------|---------|
| `index.html` | The entire site — HTML, CSS, and JS in one file |
| `resume.pdf` | Linked from the "Résumé (PDF)" button |
| `.nojekyll` | Tells GitHub to skip Jekyll processing. Don't delete it |

---

## Editing

Everything lives in `index.html`. Open it in any browser to preview locally — no server needed, just double-click it.

**Common edits:**

- **Colours** — the `:root` block at the top of `<style>`. Change `--acc` (teal) and `--acc2` (blue) to reshade the whole site.
- **Text** — search for the words you want to change. Sections are commented (`<!-- ===== EXPERIENCE ===== -->`).
- **Your math courses** — find the `edu-card math` block and edit the `<span class="tag">` entries. **I guessed at these — replace them with your actual coursework.**
- **Resume PDF** — replace `resume.pdf` with whichever tailored version you want public. Keep the filename.

---

## The audio demo

The centrepiece widget runs the real evaluation protocol from the research: greedy nearest-first one-to-one matching between predicted and ground-truth boundaries within a tolerance window, then precision, recall, and F₁ from the match counts.

Nothing is hard-coded. The metrics are computed in-browser from the boundary arrays (`GT`, `BASE`, `IMPR`) every time you move the tolerance slider. At the standard τ = 30 ms:

| Model | F₁ | Precision | Recall |
|-------|-----|-----------|--------|
| VG-HuBERT baseline | 0.4225 | 0.417 | 0.429 |
| + segmentation head | 0.7714 | 0.771 | 0.771 |

That's an 83% relative gain — the same figure on your resume, arrived at by the same arithmetic.

The waveform is synthesised from the syllable timings with a deterministic PRNG, so it renders identically on every load. Boundary sets are a faithful synthetic reconstruction — the underlying speech corpus isn't yours to publish, and the site says so explicitly under the widget.

**If an interviewer asks whether this is real data:** the protocol and the metric are real, the audio is a reconstruction. That's stated on the page. Don't let anyone leave thinking otherwise.

---

## Before you go live — checklist

- [ ] **Replace the math coursework tags** with your real courses
- [ ] Confirm "Minor" is right — the card says Minor in Mathematics. Change to "Double Major" if that's accurate
- [ ] Check both GitHub and LinkedIn links resolve
- [ ] Swap `resume.pdf` for whichever version you want public
- [ ] Open it on your phone — it's responsive, but look anyway
- [ ] Add the URL to your resume header and LinkedIn
