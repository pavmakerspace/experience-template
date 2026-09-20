# OXED Experience Template

A starting point for a new OceanX Education online learning experience:
a classroom web page (`index.html`) plus its two printable companion
documents (`educator-guide.html`, `learner-activity.html`).

Based on the [I Am Kelp](../I%20Am%20Kelp) layout as of Sep 2026, after a
round of streamlining that brought [Seagrass Stories](../Seagrass%20Stories)
onto the same system. Both those projects were built independently first
and reconciled after the fact; this template exists so the next one
doesn't have to go through that.

## Why this exists

Seagrass Stories and I Am Kelp were built separately, then made to match
one another turn by turn: header height, logo size, container widths,
button states, section rhythm, each one found by comparing screenshots
and hunting down the equivalent CSS rule in a file that was never meant
to share a system with the other. A shared template with real design
tokens means a new programme starts already consistent, and any future
fix to the shared pattern only has to happen once.

## Quick start

1. Copy this whole folder and rename it for your programme.
2. Open `styles.css` and fill in the `:root` block at the top: that's
   the whole colour system. You shouldn't need to touch a hex code
   anywhere else in the file.
3. Search all three HTML files for `[Bracketed placeholders]` and
   replace with your programme's content. Your editor's find-in-files
   will catch all of them since they all use square brackets.
4. Replace `assets/logos/` and `assets/fonts/` only if the brand
   changes. Otherwise leave them: they're the shared OceanX Education
   assets, already correct (see the "logo variants" note below).
5. Serve locally over HTTP before testing (the YouTube embeds need it):
   ```bash
   python3 -m http.server 8765
   ```
6. Delete whichever optional sections don't apply to your programme
   (each one is marked with an HTML comment in `index.html`).

## The design tokens (`:root` in `styles.css`)

| Token | Role | Default value | Source |
|---|---|---|---|
| `--abyss` | Deepest solid fill: footer, video plates | `#041738` | OceanX Blue |
| `--deep` | Strong solid accent: badges, dark section fills, primary buttons | `#002F6C` | Twilight Blue |
| `--ocean` | Secondary accent: borders, arrows, link-style text | `#0072CE` | Shoal Blue |
| `--ocean-light` | Tertiary blue: hover states, card borders | `#46B7D3` | OceanXplorer Blue |
| `--moss` | Small-caps kickers/labels | `#2DA2BF` | Explorer Dark |
| `--lime` | Highlight: CTA buttons, small pills | `#FFC846` | Warm Yellow (Education "Highlight" role) |
| `--foam` | Light near-white: text on dark sections, light section backgrounds | `#EAF7FA` | Explorer Blue, lightest step |
| `--paper` | Page background (light) | `#F1F2EF` | Sand |
| `--ink` | Body text colour on light backgrounds | `#002F6C` | Twilight Blue (design system's `--text-strong`) |
| `--muted` | Secondary/muted text | `#5B6770` | Ridge Gray (design system's `--text-body`) |
| `--line` / `--white-line` | Hairline borders on light / dark backgrounds | n/a | tinted from the above |

`--moss` was called `--kelp` in the original; renamed here since it's a
generic label-accent role, not tied to any specific programme's content.
Do the same check before naming any new token: does the name describe
its *role*, or does it describe *this programme's* subject matter? Only
the former belongs in a shared template.

These are the real OceanX Design System tokens (see `OceanX Design
System/tokens/colors.css` and `guidelines/colors-education.card.html` in
the parent folder), not I Am Kelp's own teal-green theme, which was
specific to that programme's content, not a brand default. The print
docs (`educator-guide.html`, `learner-activity.html`) use their own
small `:root` block with the same token names and the same default
values, so a fresh copy of this template is colour-consistent across
the web page and both print docs out of the box.

## Reskinning for a specific programme

If a programme wants its own thematic palette (the way I Am Kelp uses
teal-green, or Seagrass Stories uses lime-green), that's a deliberate
per-project decision: swap the `:root` values in `styles.css`. Just
know you're trading the shared brand baseline for a one-off theme, and
consider whether the print docs should follow the same theme or stay on
the OceanX brand blues (print docs are more likely to be judged against
brand guidelines directly, since they're often shared outside the web
page's context).

## Structural things not to break

- **Accordion title/badge alignment.** Each step's title text must stay
  wrapped in its own `<span>` inside `<summary>`, and that span must
  keep `flex:1 1 auto`. Without it, the "15 mins" badge drifts left or
  right depending on how long each step's title is, instead of lining
  up in a column. This was a real bug, found and fixed once already.
- **Container width parity.** The sticky header, every full-bleed
  section, and the footer should all use `.shell` (web page) for their
  inner content wrapper, matching the same `max-width`. If a section's
  content width drifts from the header's, everything above it stops
  lining up on the left edge: this was the most persistent parity bug
  between Kelp and Seagrass, and it's easy to reintroduce by accident.
- **Back-link pattern in the print docs.** Use
  `onclick="if(history.length>1){history.back();return false;}"` with
  `href="index.html"` as the fallback, not a hardcoded link. This lets
  it behave like a real back button when opened from the web page, but
  still go somewhere sensible if opened directly (e.g. a bookmark).
- **`id`s on the accordion steps** (`step-1` … `step-4`) must match the
  `data-step` values in the progress nav exactly: the scroll-spy script
  at the bottom of `index.html` depends on it, and needs zero per-project
  editing as long as this convention holds.

## What's still a manual per-project decision

- Whether to include a hero side panel (`.play-panel`) for a standalone
  game/tool companion piece: commented out by default in `index.html`,
  since I Am Kelp's actual hero doesn't have one (single column). Only
  uncomment it if this specific programme genuinely needs a CTA card
  there. If you do, note `.play-panel` uses `align-self:start` rather
  than the grid's default `align-items:end`. With it sitting low in
  the hero, scrolling gave it a long stretch where the sticky header
  partially covered it before it fully scrolled away, which looked
  broken. Starting near the top keeps that transition short.
- Whether to include Further Reading, the related-programme cross-link,
  and the feedback prompt: all marked optional in `index.html`.
- The Create-phase illustration in `learner-activity.html`: bring your
  own, or leave it out entirely and let the bordered canvas + corner
  prompts stand alone.
- Whether your programme's outcomes are Do/Think/Feel, or something
  else: the `.outcomes` column can be replaced with a single paragraph
  if Do/Think/Feel doesn't fit.

## Not part of this template

This only covers the "Explore · Investigate · Reflect · Create" journey
pattern used by this engagement's online experiences. It doesn't cover
print-only deliverables (like Education Booklets), slide decks, or
programmes without a web component: those need their own structure.
