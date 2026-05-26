# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

Marketing/pitch material for an AI-First Instructional Design Platform. Each top-level `*.html` file is a self-contained presentation deck (investor pitch, user story narrative, process diagrams) hosted via GitHub Pages at `https://booya1986.github.io/AI-First-Instructional-Design-Platform/<file>.html`.

There is no build step, no package manager, no tests. The "app" is the HTML files themselves.

## Critical: .gitignore Allowlist

`.gitignore` is configured as **ignore-everything-except** — it starts with `*` and explicitly un-ignores specific files:

```
*
!.gitignore
!investor_deck.html
!user_story_deck.html
!user_story_deck_he.html
!needs_analysis_flow.html
!README.md
!img/
!img/**
!skills/
!skills/**
```

**Any new HTML file you create will be silently ignored by git.** When adding a new deck, you must also add a `!new_deck.html` exception to `.gitignore` in the same commit. Confirm with `git status` that the file appears as untracked before committing.

## Architecture

### Single-file HTML decks
Every deck embeds all CSS and JavaScript inline. No external dependencies except Google Fonts. This is a deliberate constraint — keep it that way. A deck must work when downloaded and opened locally.

### Slide system
The two presentation decks (`investor_deck.html`, `user_story_deck.html`, `user_story_deck_he.html`) use a slide-based architecture: a `#deck` container, `.slide` elements with `data-slide` attributes, `.slide.active` class for the visible one, and vanilla JS for keyboard/touch/button navigation. `needs_analysis_flow.html` is a one-pager (no slides) but shares the same design tokens.

### Bilingual pairing
`user_story_deck.html` (English LTR) and `user_story_deck_he.html` (Hebrew RTL) are paired. Hebrew version uses `dir="rtl"`, Noto Sans Hebrew font, and `[dir="rtl"]` CSS overrides rather than a duplicated stylesheet. Character names are translated (Maria→רוני, David→דוד, Sarah→שרה, Jake→ירון). When changing one, evaluate whether the other needs the same change.

## Design System (Claude Code Dark)

Authoritative reference: `sildeshow deck design principelce.md` (yes, the filename is misspelled — don't rename it, things may link to it). Implementation patterns: `skills/frontend-engineer/`.

**Non-negotiable tokens** — every deck must use these exact values:
```css
--bg-base: #0B0B0B;              /* page background */
--bg-elevated: #111111;          /* cards */
--text-primary: #FFFFFF;
--text-secondary: rgba(255,255,255,0.72);
--text-tertiary: rgba(255,255,255,0.56);
--border: rgba(255,255,255,0.12);
--dot-grid: rgba(255,255,255,0.08);  /* fixed background pattern */
```

**Fonts:** Noto Sans Hebrew for Hebrew decks; Inter + JetBrains Mono for English decks.

**Core principle: monochrome discipline.** Greyscale only — no color accents. If you find yourself reaching for purple/cyan/green/etc., stop and use white at varying opacities instead. Data viz uses 7-step greyscale `#FFFFFF → #6C6C6C`.

**Layout:** 1440×900 base artboard, 64px margins, 24px gutters. Breakpoints: tablet 768–1024px, mobile <768px.

## GitHub Pages

`main` branch is the deploy branch. Pushing to `main` deploys live. There is no preview environment — verify changes locally by opening the HTML file in a browser before pushing.

## Conventions

- **Commit style:** short imperative subject, optional body explaining *why* (see `git log` for examples). All commits include the `Co-Authored-By: Claude` trailer.
- **No README updates for new decks** unless explicitly asked — README is hand-curated, not a deck index.
- **RTL gotcha:** in RTL Hebrew, `inset-inline-end` is the left side and `inset-inline-start` is the right side. Use logical properties; don't hardcode `left`/`right`.
