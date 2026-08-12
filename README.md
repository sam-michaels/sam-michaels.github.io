# sam-michaels.github.io

Personal portfolio site. Single `index.html`, no framework, no build step.

**Live at:** https://sam-michaels.github.io

---

## What's in here

| File | Purpose |
|------|---------|
| `index.html` | The entire site — HTML, CSS, JS, **and the full résumé** in one file |
| `resume.pdf` | Optional. Powers the "Download PDF" button in the Résumé section |
| `.nojekyll` | Tells GitHub to skip Jekyll processing. Don't delete it |
| `.gitignore` | Keeps editor lock files and temp files out of the repo |

**The résumé is inline.** The `#resume` section renders the whole thing as HTML, so the nav link and the footer button can never 404 — they're same-page anchors. Recruiters can read it without downloading anything, and search engines can index it.

`resume.pdf` is a convenience on top of that. If you don't upload it, the page detects the missing file on load, hides the download button, and shows a one-line note instead. Nothing breaks.

### "I clicked Résumé and got a 404"

That was the old behaviour, when the button pointed straight at `resume.pdf`. It's fixed — the button now scrolls to the inline section.

If the **Download PDF** button 404s, it means `resume.pdf` didn't make it into the repo. Check with:

```bash
git ls-files | grep resume
```

No output means it was never committed. Fix:

```bash
git add -f resume.pdf
git commit -m "Add resume PDF"
git push
```

The `-f` matters if a global gitignore excludes PDFs — that's the usual culprit.

---

## Editing

Everything lives in `index.html`. Open it in any browser to preview locally — no server needed, just double-click it.

**Common edits:**

- **Colours** — the `:root` block at the top of `<style>`. Change `--acc` (teal) and `--acc2` (blue) to reshade the whole site.
- **Text** — search for the words you want to change. Sections are commented (`<!-- ===== EXPERIENCE ===== -->`).
- **Your math courses** — find the `edu-card math` block and edit the `<span class="tag">` entries.
- **Resume PDF** — replace `resume.pdf` with whichever tailored version you want public. Keep the filename.
- **Theme** — the site respects the visitor's OS preference on first load, then remembers their choice in `localStorage`. To force one, change `data-theme="dark"` on the `<html>` tag.

---

## Views and theming

The demo has three views — **Waveform**, **Spectrogram**, **Both** (default). The chosen view and the light/dark theme both persist in `localStorage`, so a returning visitor gets what they left on.

The spectrogram is a real short-time Fourier transform computed in the browser: a source-filter speech signal is synthesised at 8 kHz from the syllable timings (harmonic glottal source, three formants interpolating between vowel targets, broadband onset transients), then windowed with a 512-point Hann window at a 128-sample hop and run through a hand-written radix-2 FFT. Magnitudes go to dB, get mapped through a theme-aware colour ramp, and land in an offscreen bitmap.

That costs about 200 ms, so it never blocks first paint: the page renders the waveform immediately with a `computing STFT…` placeholder, defers the transform to the next macrotask, and redraws when the data arrives.

**What to point at in an interview:** the horizontal bands are formants, the fine striations are harmonics of F₀, and the syllable boundaries are the vertical discontinuities — broadband onset burst followed by a jump in formant trajectory. That is the acoustic evidence the model has to localise.

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

- [ ] Confirm "Minor" is right — the card says Minor in Mathematics. Change to "Double Major" if that's accurate
- [ ] Click the theme toggle and check both themes on your own screen
- [ ] Check both GitHub and LinkedIn links resolve
- [ ] Swap `resume.pdf` for whichever version you want public
- [ ] Open it on your phone — it's responsive, but look anyway
- [ ] Add the URL to your resume header and LinkedIn
