# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Two **independent, self-contained LaTeX tutorials** for data/ML engineers who use Python and need to be productive in a shell:

- `Bash-WSL2-Tutorial.tex` — primary tutorial (bash on WSL2/Ubuntu), ~1900 lines, 7 parts.
- `PowerShell-Tutorial.tex` — secondary reference (PowerShell on Windows), ~1400 lines, 5 groups.

There is no application code, test suite, or shared `.sty`/`\input` file. Each `.tex` is a single source compiled to its sibling `.pdf`. The two documents cross-reference each other in prose but build separately.

## Build

Run `pdflatex` **twice** per file so the table of contents resolves (any TeX Live or MiKTeX install works):

```powershell
pdflatex -interaction=nonstopmode Bash-WSL2-Tutorial.tex
pdflatex -interaction=nonstopmode Bash-WSL2-Tutorial.tex

pdflatex -interaction=nonstopmode PowerShell-Tutorial.tex
pdflatex -interaction=nonstopmode PowerShell-Tutorial.tex
```

No `--shell-escape`, no `.bib`/bibliography, no external figures. Required packages (all standard, bundled with TeX Live/MiKTeX): `listings`, `xcolor`, `hyperref`, `geometry`, `setspace`, `parskip`, `booktabs`, `array`, `enumitem`, `tcolorbox`. The `.aux`/`.toc`/`.out`/`.log` artifacts are gitignored.

## Editing conventions (the non-obvious parts)

Each file duplicates a similar preamble, but the macros and styles are **file-local and not identical** — do not copy snippets between the two `.tex` files without translating:

- **`\cmd` means different things in each file.** In `Bash-WSL2-Tutorial.tex`, `\cmd` colors text as a bash keyword; the Python/PowerShell inline macros there are `\pcmd` and `\pscmd`. In `PowerShell-Tutorial.tex`, `\cmd` colors text as a PowerShell keyword and `\bcmd` is the bash one. Pick the macro by the *current* file, not by the language.

- **Every `lstlisting` must name its style explicitly**, e.g. `\begin{lstlisting}[style=bashstyle]`, `[style=pystyle]`, or `[style=pwshstyle]`. Both languages appear interleaved in the same document, so relying on the `\lstset` default (bashstyle in the bash doc, pwshstyle in the PowerShell doc) will mis-highlight a block.

- **PowerShell has no built-in `listings` language.** Each file defines its own via `\lstdefinelanguage`, and the two use *different names*: it's `PowerShellAlt` in `Bash-WSL2-Tutorial.tex` but `PowerShell` in `PowerShell-Tutorial.tex`. To get a new cmdlet syntax-highlighted, add it to the `morekeywords` list of that file's definition (the PowerShell file's list is already fairly complete).

- **Callout boxes are `tcolorbox` environments** defined per file: the bash doc has `comparepy`, `compareps`, and `gotcha`; the PowerShell doc has `compare` and `gotcha`. Use these for cross-language notes and warnings rather than ad-hoc boxes.

- Document structure uses unnumbered `\part*{...}` for the top-level parts and `\section{...}` within. The bash doc opens with `\maketitle`, `\begin{abstract}`, `\tableofcontents`.

## Content style

Worked examples use realistic data (CSV rows, JSON API responses, log files), not `foo`/`bar`. Most sections include a "Compare with Python" angle, and Part VII / the Reference group of each tutorial ends with a cross-language lookup/translation table — keep new material consistent with that framing.
