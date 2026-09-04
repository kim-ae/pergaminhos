# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is **Pergaminhos**, an [Obsidian](https://obsidian.md/) vault used as kim7s's personal knowledge base. There is no source code to build, lint, or test — all work here is reading, writing, and organizing Markdown notes, Obsidian canvases (`.canvas` JSON files), and a few images. Do not look for or propose build/lint/test tooling; none applies.

Git is used purely for backup/versioning of the vault. Commits are automated snapshots with messages like `vault backup: <timestamp>` — do not read into commit granularity or try to craft conventional-commit-style messages unless the user asks for a specific one.

## The kim7s knowledge model — apply this whenever documenting something

**Any time the user asks to document, write up, or take notes on something in this vault, it MUST be organized per this model.** It is authoritatively defined in `kim7s knowledge model/` (`Section.md`, `Subsection.md`, `Get started.md`, `Get Knowledgeable.md`, `Get Help.md`, `zSource.md`, `CHANGELOG.md`) — treat that folder as the spec, not just background reading.

### Structural units

- **Section**: a top-level folder for a broad subject — a whole technology, complex tool, language, framework, or theory area (e.g. `K8S`, `ELK`, `Linux`). A well-formed Section contains:
  - `0Tips.md` — flat file of quick, tag-heavy snippets. (See Tips below.)
  - a `Get Started` folder — Onboarding + First Look.
  - a `Get Knowledge` folder — Theory + How-To.
  - a `Get Help` folder — Troubleshoot + References + Tips-in-depth.
  - `zSource.md` — links to official manuals/docs for the subject (the `z` prefix sorts it last).
- **Subsection**: a folder nested under a Section, for a sub-tool or component of that Section (e.g. `ELK/Filebeat`, `ELK/ECK`). Identical to a Section except it has **no** `0Tips.md` and no tags file.
- A previously-defined `1Tags` file (a centralized tag registry) was deliberately removed — see the CHANGELOG: tags are only useful for searching within docs, not as a standalone list. **Do not recreate a centralized tags file.** Tags stay inline as hashtags (e.g. `#pod`, `#kubectl`, `#debug`) on individual notes.
- In practice, numbering prefixes drift across existing sections (e.g. `Linux` uses `0Get Started/1Get Knowledge`; `K8S`/`ELK`/`Eng SW`/`Grafana` use `1Get Started/2Get Knowledge/3Get Help`). When editing an existing Section/Subsection, match its existing numbering rather than the canonical `2/3/4` from `Section.md`. When creating a brand new Section, use `1Get Started/2Get Knowledge/3Get Help` (the convention already used by the most recently created sections).

### Deciding where new content goes (the actual decision procedure)

When asked to document something, classify the content by what it *is*, not by which tool it's about, then place it accordingly:

1. **Quick command, snippet, or one-liner with minimal explanation, meant to be found by tag** → `0Tips.md` (Section-level only; Subsections don't get their own Tips file — put subsection tips in the parent Section's `0Tips.md`, tagged for the subsystem). No theory, no tutorial, ever.
2. **Steps to get the tool/tech installed, configured, and onboarded for the first time, no explanation of internals** → `Get Started/Onboarding`.
3. **"What do I do first" walkthrough — first real usage of the tool, still no deep theory** → `Get Started/First Look`.
4. **Conceptual/theoretical explanation of how something works — technical, mathematical, or design theory, tool-agnostic** → `Get Knowledge/Theory`.
5. **Tutorial on performing a specific task, built on top of the theory and tools already introduced** → `Get Knowledge/How-To`.
6. **A troubleshooting case: symptom, investigation, root cause, fix** → `Get Help/Troubleshoot`.
7. **Pointer/summary of official documentation or manuals (not original content)** → `Get Help/References`, or `zSource.md` if it's just a link with no summary.
8. If the topic doesn't yet exist as a Section or Subsection, create the full folder structure above rather than dropping a loose file at the vault root.

If the requested content spans categories (e.g. "document this outage" produces both a Troubleshoot writeup and a new Tip), split it across the right files instead of writing one combined note.

## Top-level layout

- `kim7s knowledge model/` — the meta-documentation defining the structure above. Treat as source of truth for organizational conventions.
- `K8S/`, `ELK/`, `Grafana/`, `Azure/`, `Linux/`, `Eng SW/` — Sections for specific technologies, following the model above. `ELK/` further contains `ECK/` and `Filebeat/` as Subsections.
- `Studies/` — looser, in-progress notes grouped by broad area (`AI`, `Development`, `Ops`, `My Home Lab`) that don't yet fully conform to the Section/Subsection model.
- `Projects/` — scratch/idea space for personal projects (currently just an Obsidian canvas sketching ideas, no implementation).
- `Ideas.md`, `Skills.md` — loose personal scratch notes at the vault root, not part of any Section.
- `.obsidian/` — Obsidian app configuration (workspace layout, plugins). Not content; avoid editing unless the user is specifically asking to change Obsidian settings.

## Working conventions

- `.canvas` files are Obsidian JSON canvases (node/edge graphs referencing other notes or free text). Edit them as JSON, preserving the existing node/edge id scheme — don't regenerate ids for unrelated nodes.
- When asked to add a note, place it under the correct category (Tips vs Get Started vs Get Knowledge vs Get Help) based on the definitions above, not just topical relevance — the category is the whole point of the model.
