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

## Where it is deployed

**Live: <https://portfolio.muhammadtilal.workers.dev>**

- **Source:** <https://github.com/mtilal/portfolio> (branch `main`)
- **Host:** Cloudflare Workers with static assets, named `portfolio`, connected
  to the GitHub repo. Every push to `main` redeploys automatically in about
  twenty seconds.
- **Cost:** free, permanently. Cloudflare's free tier covers this many times
  over, and the site is under 500 KB in total.

The URL is `<worker-name>.<account-subdomain>.workers.dev`. Both halves are set
in the Cloudflare dashboard: the worker name under the worker's own settings,
the account subdomain under the account. Renaming the worker changes the URL
immediately, and the old one stops resolving.

`_headers` is honoured here exactly as it is on Pages — assets are cached at
the edge for a year while the HTML always revalidates, so an edit is visible
at once but repeat visitors re-download nothing.

### Pointing a custom domain at it

A domain is the only part that costs money — roughly $11/year at any
registrar; the hosting stays free. In the Cloudflare dashboard open the worker,
then **Settings → Domains & Routes → Add → Custom domain**. HTTPS is issued
automatically. The `workers.dev` URL keeps working alongside it, so anything
already printed on a CV stays valid.

Afterwards, update the seven places the site names its own address: the
canonical link, the two `og:` tags and the JSON-LD block in `index.html`, plus
`sitemap.xml` and `robots.txt`. Those only affect Google's listing and link
previews on LinkedIn or WhatsApp; the site works either way.

---

## Changing anything, later

The easiest route needs no local setup at all: open `index.html` on GitHub,
click the pencil, use Ctrl+F to find the sentence, retype it, and hit **Commit
changes**. Cloudflare redeploys within about twenty seconds.

To edit locally instead, change the file and run these — one per line, because
Windows PowerShell does not accept `&&` as a separator:

```bash
git add .
```
```bash
git commit -m "Describe the change"
```
```bash
git push
```

Every deploy is kept, so you can roll back to any earlier version from the
Cloudflare dashboard.

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

## A free fallback host, if ever needed

Nothing about this site is tied to Cloudflare — it is plain static files. It
will run unchanged on GitHub Pages (repository **Settings → Pages**, source
*Deploy from a branch*, branch `main`, folder `/ (root)`), on Netlify, or on
any static host. GitHub Pages ignores `_headers`, so you would lose the edge
caching rules and the security headers; everything else behaves identically.
