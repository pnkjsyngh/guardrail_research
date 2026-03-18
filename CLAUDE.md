# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project purpose

Documentation-only research project producing a white paper on LLM/AI guardrail systems. Full plan, schedule, vendor list, and methodology are in [`RESEARCH_PLAN.md`](RESEARCH_PLAN.md).

## Repository layout

```
research/
  sources/<vendor>/raw/   raw downloaded docs (PDF/HTML) with retrieval date in filename
  sources/<vendor>/README.md  citation log: exact URLs and access dates
  notes/<vendor>.md       filled System Profile (use research/templates/system_profile.md)
  templates/              reusable templates — do not edit without good reason
paper/
  guardrails_whitepaper.md  final manuscript
  STRUCTURE.md            required section outline and placeholders
  references.bib          BibTeX — one entry per source referenced in the paper
  figures/                image assets
```

## Behavioral rules

- **Documentation-only.** Do not attempt live vendor API calls under any circumstances. All analysis is based on publicly available documentation, vendor examples, and SDK snippets.
- **Mark assessment type clearly.** Label each vendor's profile as `documentation-only` at the top of `research/notes/<vendor>.md`.
- **Cite everything.** Every claim in the paper must trace to a file in `research/sources/` and a key in `paper/references.bib`. Do not add uncited assertions.
- **No secrets in the repo.** Credential notes go in a local `secrets.md` that is `.gitignore`d.

## Markdown-for-.docx conventions

The white paper will be converted to `.docx` via Pandoc. Follow these rules when writing or editing `paper/guardrails_whitepaper.md`:

- ATX headings only (`#`, `##`, `###`); do not skip levels.
- Figures in `paper/figures/`; reference with `![Alt text](figures/name.png)` followed by a plain-text caption paragraph.
- Fenced code blocks with language tags (` ```python `).
- Simple Markdown tables; avoid nested or complex markup.
- In-text citations use Pandoc citeproc style: `[@vendor2024]`.
- No raw HTML blocks.
- Complex layouts (multi-column, side-by-side figures) are deferred to post-conversion `.docx` editing.
