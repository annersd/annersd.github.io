# CLAUDE.md — annersd.github.io

Orientation for an LLM (or human) working on this repo. It is deliberately small:
this is a **single-page static site**, not an application.

## What this is

The organisation landing page for **annersd**, served by GitHub Pages at
<https://annersd.github.io/>. It introduces the org, its stance, and the projects
it publishes.

## Layout

```
index.html        the entire site — one self-contained page (inline CSS, no JS)
assets/walnut.png the mark: hero avatar, favicon and Open Graph image
.nojekyll         disables Jekyll; files are served exactly as committed
CLAUDE.md         this file
```

## Deployment

- GitHub Pages, **Deploy from a branch** → `main` / `(root)`.
- A push goes live in roughly a minute. GitHub's CDN caches aggressively — verify
  with a cache-busting query (`?cb=123`) or a hard reload before assuming a
  deploy failed.
- Because of `.nojekyll`, **every committed file is publicly fetchable** as-is
  (including this one). Never commit anything here that shouldn't be public.

## Conventions & invariants (do not break)

1. **Self-contained.** One HTML file, inline `<style>`, no build step, no external
   fonts, scripts or CDNs. It must render correctly offline from the file system.
2. **Theme-aware.** Dark by default, light via `prefers-color-scheme`. The palette
   is warm (amber/copper) and derived from the walnut mark — keep new colours in
   that family and drive them through the CSS variables in `:root`.
3. **No links into private repositories.** `conduit-for-claude-code` is currently
   private, so any link to it would 404 for visitors. It is presented inline with
   a `Private preview` chip instead. Remove the chip and add links only once that
   repo is actually public.
4. **No LAN or local URLs** (`192.168.*`, `localhost`). Assets must be committed
   under `assets/` and referenced relatively, or they break for every visitor.
5. **No private identifiers.** Keep real names, internal project names and other
   material from private notes out of this repo.
6. **Commits: never** add a `Co-Authored-By` trailer for Claude (or any AI), and
   never name an AI as author/co-author — the human contributor is the sole author.
7. **History is kept as a single commit.** Changes are folded in with
   `git commit --amend` followed by `git push --force-with-lease origin main`.

## Page structure

| Section | Content |
|---|---|
| **Hero** | walnut mark · `annersd` · tagline *"Making things annersd."* · lead · the motivation pull-quote · a reference line (Forbes article + GitHub) |
| **Approach** | the manifesto paragraph + three chips (Fresh eyes · Unorthodox approaches · New solutions to old problems) |
| **Projects** | one compact entry per project |

**Brand note:** *annersd* is used as the name and as the verb in the tagline. The
word's dialect origin is deliberately **not** explained on the page — the meaning
is carried by the tagline and the lead instead.

## Colour scheme

Every colour is a CSS custom property on `:root`, overridden wholesale inside
`@media (prefers-color-scheme: light)`. **Never hard-code a colour in a rule** —
add or reuse a token instead, so both themes stay in sync.

| Token | Dark (default) | Light | Role |
|---|---|---|---|
| `--bg` | `#100c09` | `#fffcf7` | page background (warm near-black / warm white) |
| `--bg2` | `#17110c` | `#faf4ea` | raised surface: manifesto, chips, tag rows |
| `--card` | `#1b1410` | `#ffffff` | cards (project block) |
| `--border` | `#2e241b` | `#e8dccb` | 1px hairlines, section dividers |
| `--fg` | `#f4ece2` | `#241a12` | body text |
| `--muted` | `#a89279` | `#6d5c4a` | secondary text, labels, tags |
| `--accent` | `#e2a45f` | `#a3611f` | links, emphasis, left accent borders |
| `--accent2` | `#c9743a` | `#82461a` | **second gradient stop only** |

- The palette is **warm amber/copper, derived from the walnut mark**. A cool or
  blue accent clashes with the photograph — keep new colours in this family.
- Light-mode accents are deliberately **darker** than their dark-mode counterparts
  so links stay legible on white. Never reuse a dark-mode value in light mode.
- Gradients are always `linear-gradient(90deg, var(--accent), var(--accent2))`
  (the wordmark) and tinted glows use
  `color-mix(in srgb, var(--accent) N%, transparent)` — so they follow the theme
  automatically.

## Layout & type

Single column, no framework, no media queries beyond the colour scheme —
fluid sizing does the responsive work.

- **Container:** `.wrap` → `max-width: 880px`, `padding: 0 20px`, centred.
- **Vertical rhythm:** hero `72px / 60px`, every `section` `56px 0`, footer
  `38px 0 64px`. Sections are separated by 1px `--border` hairlines, not by
  background changes.
- **Type:** body `16px / 1.65`, system font stack. Headings scale fluidly with
  `clamp()` — `h1` `clamp(2.4rem, 8vw, 3.9rem)`, `.tagline`
  `clamp(1.15rem, 3vw, 1.5rem)`, `.quote` `clamp(1.1rem, 2.8vw, 1.42rem)`.
  `h2` is a small uppercase label (`1rem`, `letter-spacing: .08em`, `--muted`).
- **Measure:** text blocks are capped in `ch` so lines stay readable — `.lead`
  `52ch`, `.quote` `40ch`, `.pdesc` `66ch`, `.manifesto` `68ch`. Keep these caps
  when adding copy.
- **Shapes:** radius `14–16px` for cards and the manifesto, `999px` for chips,
  `50%` for the avatar. Borders are always `1px solid var(--border)`.
- **Hero mark:** `132px` circle with a soft drop shadow plus a tinted ring
  (`box-shadow` with `color-mix`), `object-fit: cover` so any square source works.
- **Density rule:** the hero carries the message; everything below it stays
  compact. The Projects block is intentionally about a quarter of a screen — see
  the recipe below.

## Recipes

- **Add a project** → copy the `.project` block in the Projects section. Keep it
  compact: heading + optional status chip, a two-line description, and one muted
  `.tags` line. Resist giving individual features their own cards — that block is
  intentionally about a quarter of a screen.
- **Change the mark** → replace `assets/walnut.png` (it is clipped to a circle via
  `border-radius: 50%`, so a square source is fine) and keep the `og:image` and
  `icon` references pointing at it.
- **Add a reference link** → extend the `.refline` in the hero. Only link URLs that
  actually resolve publicly; never invent a citation URL.
