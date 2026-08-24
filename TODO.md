# Pandorax — outstanding work

Deferred items, roughly in impact order. Everything here was found by measuring
the site, not guessed — the numbers are current as of 2026-08-21.

## Done

- ~~**Page weight**~~ — index 19 MB → **3.5 MB**, team 39 MB → **3.1 MB**.
  56 fully-opaque portraits and the collage converted to JPEG; the rest resized.
- ~~**Alt text**~~ — 128 attributes added, 0 images now missing one.
- ~~**Keyboard focus states**~~ — `:focus-visible` rules in all 13 stylesheets.

Two encoder traps worth remembering for any future image pass:

- `sips` re-encodes PNG *less* efficiently than whatever produced the originals.
  Five files grew when resized and had to be restored from git.
- `Image.quantize()` wrecks an alpha channel. It cut `indbg4.png` from 78%
  fully-opaque to 1.8%, leaving 77% of pixels partially transparent — the image
  would have rendered washed out. Palette PNGs must have their alpha snapped
  back to binary after resampling, and the result checked.

Always diff before/after size *and* the alpha histogram; do not assume a resize
or re-encode is an improvement.

## 1. Five bootcamps share one sign-up form

- `forms.gle/mtejqPZudVmxtg7v9` — NACLO, USABO, USAPhO, USNCO, USAAAO
- `forms.gle/kDx1hvMovjz9M5Qy8` — ML, USESO

If those forms do not ask which bootcamp the person wants, registrations cannot
be told apart.

## 2. Two team photos 404 in production

`pplimg/zelmay.png` (referenced twice) and `pplimg/yufei chen.png`. Either supply
the files or remove the cards.

## 3. Missing headshots

Six bootcamp instructors still render without a photo: Hubert Lau, Owen Zhou,
Anish Agarwal, Ryan Miao, Aariz Anas, Dun Li Chan. Akhil Batchu and Allison Zhou
use the generic `pfp.png` placeholder.

Export around 800px wide — the recent additions are 143–187px, which get
upscaled 75–130% on the bootcamp cards and look soft.

## 4. `class="containter2"` typo — `index.html:29`

The rule is `.container2`, so it has never applied. Correcting it would newly
activate `max-width: 100%` and `overflow-x: hidden` on that container, which is a
layout change rather than a no-op. Needs a browser check, not a blind fix.

## 5. Repo hygiene

- No `.gitignore`. Three `.DS_Store` files and `log.csv` are tracked and served
  publicly — `pandorax.org/log.csv` returns 200.
- `docs/CNAME` is a dead duplicate of `CNAME`.
- Roughly 90 MB in `assets/webstuff/` is referenced by nothing at all
  (`tibleb2.gif` 39 MB, `tibleb.mp4` 23 MB, and others). The deploy workflow
  uploads `path: '.'`, so all of it ships.

## 6. Invalid CSS in the nav

12 of the 13 `.head` rules contain `align-items: under` and a duplicate
`justify-content: space-between 4px`. Browsers drop both, and the current layout
depends on them being dropped — so this is a cleanup to do deliberately with a
browser open, not a quick fix. `impact.css` is already clean.
