# Portfolio — Muhammad Tilal

A single-page portfolio. No framework, no build step, no JavaScript at all —
one HTML file plus four assets. Double-click `index.html` to see exactly what
goes live.

```
index.html                    the whole site: markup + styles
assets/inter.woff2            47 KB  — body text
assets/spectral.woff2         22 KB  — display headings
assets/portrait.jpg           21 KB
assets/CV-Muhammad-Tilal.pdf 343 KB  — replace this file to update the CV
favicon.svg                   browser-tab icon
_headers                      caching + security headers (Cloudflare reads it)
robots.txt, sitemap.xml       so search engines index the page
```

**First load: ~98 KB.** The page compresses to under 10 KB; the rest is two
fonts and the photo. The CV is not included in that — it downloads only when
someone clicks the button.

---

## The design

White ground, navy ink, one blue accent. Spectral (a screen-optimised serif)
sets the name, section titles and figures; Inter sets everything else. Nothing
is pure black — body copy is `#17375e`, which is easier on the eye over long
stretches than black on white.

Sections run: hero → at-a-glance (skills and education, above the fold) →
selected delivery work → skills and AI tooling → experience → education and
certifications. Skills sits high deliberately: it is what a recruiter scans
for. Bands alternate white and a pale blue-grey tint to give rhythm without
drawing a border round everything.

There is no contact section at the foot. Every contact route is a button in
the hero instead — Download CV, LinkedIn, GitHub, email and phone — so nobody
has to reach the bottom of the page to find them. On a phone the CV button
spans the row and the other four pair up two-per-row, with short labels
("Email", "Phone") swapped in so they fit a half-width button.

Running prose is justified with hyphenation switched on (`hyphens: auto` plus
`hyphenate-limit-chars`). The hyphenation is what makes justification work —
without it the browser can only stretch word spaces, which opens rivers of
white down a narrow column. Headings, dates and short labels stay ragged.

---

## Deploying it — Cloudflare Pages (free)

Free permanently at this size: unlimited bandwidth, unlimited visitors, HTTPS,
and roughly 300 edge locations, so a recruiter in London or Dubai is served
from near them rather than from Pakistan.

**1 · Put the files on GitHub**

Create a repository at <https://github.com/new> — name it `portfolio`, keep it
**Public**, add nothing else. Then from this folder:

```bash
git init && git add . && git commit -m "Portfolio site"
```

```bash
git branch -M main && git remote add origin https://github.com/YOUR-USERNAME/portfolio.git && git push -u origin main
```

**2 · Connect Cloudflare Pages**

1. Sign up free at <https://dash.cloudflare.com/sign-up> — no card required.
2. **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
3. Authorise GitHub and pick the `portfolio` repository.
4. Leave every build setting empty:
   - Framework preset: **None**
   - Build command: *(blank)*
   - Build output directory: `/`
5. **Save and Deploy.**

A minute later it's live at `https://<project-name>.pages.dev`. You choose
`<project-name>` in step 3 — take `muhammadtilal` if it's free.

**3 · One edit after the first deploy**

Replace `muhammadtilal.pages.dev` with your real address wherever it appears
in `index.html` (the canonical link, the two `og:` tags, and the block at the
bottom of the file). These only affect how the link previews on LinkedIn or
WhatsApp and how Google lists you — the site itself works either way.

---

## Changing anything, later

Edit `index.html`, then:

```bash
git add . && git commit -m "Describe the change" && git push
```

Cloudflare redeploys in about 20 seconds and keeps every previous version, so
you can roll back from the dashboard at any time.

**To update the CV:** replace `assets/CV-Muhammad-Tilal.pdf`, keeping the
filename, and push. The download button keeps working, no code change.

**To change a contact detail:** the five hero buttons hold them. Each is a
plain link in the `hero-actions` block near the top of the body — `mailto:`
for email, `tel:` for the phone. Change the `href` and the visible text
together; the phone and email buttons each carry a long label for desktop and
a short one for phones.

**To change colours:** every colour resolves to a token in the `:root` block
near the top of `index.html`. `--ink` is the navy, `--accent` the blue,
`--paper-tint` the pale band behind alternating sections. Change one and it
updates everywhere it's used.

**To add a role:** copy an `<article class="role">` block in the Experience
section. Each holds a date, an `Industry`/`Academia` tag, a title, an
organisation line and a bullet list.

**To adjust spacing:** `--band` controls the gap between sections, `--gutter`
the page margins, `--maxw` the content width.

---

## Why it loads fast

- **One request for the page.** The CSS lives inside `index.html`; there is no
  stylesheet to fetch and no JavaScript anywhere on the page.
- **Both fonts are self-hosted, not loaded from Google Fonts.** A Google Fonts
  link costs a DNS lookup and a TLS handshake to a second domain before a font
  can even begin downloading. Same-origin skips both. Each file is subset to
  Latin and preloaded so it downloads alongside the HTML.
- **The portrait is sized for its slot** and carries explicit dimensions plus
  an `aspect-ratio`, so nothing shifts as it loads.
- **`_headers` caches assets at the edge for a year** while forcing the HTML to
  revalidate — your edits appear at once, repeat visitors re-download nothing.

No trackers, no third-party requests, no cookie banner.

---

## If you'd rather use GitHub Pages

Simpler, slightly slower, also free and permanent. Do step 1, then in the
repository go to **Settings → Pages**, set **Source** to *Deploy from a
branch*, branch `main`, folder `/ (root)`, Save. It appears at
`https://YOUR-USERNAME.github.io/portfolio/` within a couple of minutes.
GitHub Pages ignores `_headers`; everything else behaves the same.

Naming the repository `YOUR-USERNAME.github.io` instead gives you the shorter
`https://YOUR-USERNAME.github.io/`, which reads better on a CV.

## A custom domain, if you want one later

Both hosts attach one free — you'd pay only the registrar for the name, about
$10–15/year for something like `muhammadtilal.com`. Hosting stays free. Add it
under **Custom domains** in the Cloudflare Pages project; HTTPS is automatic.
