# Bash on WSL2 for the Data & ML Engineer

A from-zero, ~36-page LaTeX tutorial that takes you from *nothing installed*
to *daily-driver bash on Linux* through Windows Subsystem for Linux 2
(WSL2). Assumes only that you know Python.

## Who this is for

You are a Data Engineer or ML Engineer (Python, cron, uv, AWS, MongoDB,
MySQL) who needs to be productive on Linux because that's where every
production pipeline, Docker container, CI runner, and ML training rig
actually runs. You're on a Windows laptop and want a real Linux
environment without dual-booting.

## What's inside

Twenty sections in seven logical parts:

- **Part I — WSL2 Setup & Mental Model.** Installing Ubuntu via
  `wsl --install`, the Linux filesystem and *why your code lives in
  `~/projects`, not `/mnt/c/`*, VS Code's Remote-WSL extension.
- **Part II — Bash Fundamentals.** Navigation and globbing, the
  stdin/stdout/stderr/pipe model, variables, quoting and parameter
  expansion, the `grep` / `sed` / `awk` / `sort` / `uniq` toolkit with a
  worked "top 5 IPs in a log" pipeline.
- **Part III — Data Wrangling.** CSV with `awk`, `miller (mlr)`, and
  `csvkit`/`csvsql`; JSON with `jq`; HTTP and paginated APIs with
  `curl + jq`.
- **Part IV — Environment and Python.** `export`, `.bashrc`, the standard
  `.env` secrets pattern (`set -a; source .env; set +a`), Python on Ubuntu,
  `uv` for venv and package management.
- **Part V — The Data / ML Ecosystem.** Docker on WSL2 (Docker Desktop +
  `docker compose`), CUDA / GPU passthrough for PyTorch with the
  `nvidia-smi` sanity check, install + connect commands for AWS CLI v2,
  `mongosh`, and the `mysql` client.
- **Part VI — Scripting and a Real ETL.** `set -euo pipefail` discipline,
  conditionals, loops, `trap`, CLI argument parsing, then a complete
  `enrich.sh` that ties CSV input + `curl` enrichment + `jq` + `uv run`
  Python + JSON output into one script.
- **Part VII — Reference.** Bash footguns checklist, plus a three-column
  lookup table (**Python concept ↔ Bash ↔ PowerShell**) for cross-reference.

## Files

| File | Description |
|---|---|
| `Bash-WSL2-Tutorial.tex` | LaTeX source (~60 KB, single self-contained file) |
| `Bash-WSL2-Tutorial.pdf` | Rendered output (~36 pages, ~400 KB) |
| `Bash-WSL2-Tutorial.aux` / `.log` / `.out` / `.toc` | Standard `pdflatex` aux files |

## Build

Compile with any TeX Live or MiKTeX install. Two passes are needed for the
table of contents and cross-references to resolve:

```powershell
cd C:\Users\shiru\Desktop\bash
pdflatex -interaction=nonstopmode Bash-WSL2-Tutorial.tex
pdflatex -interaction=nonstopmode Bash-WSL2-Tutorial.tex
```

Only standard TeX Live packages are used (`listings`, `xcolor`, `hyperref`,
`geometry`, `setspace`, `parskip`, `booktabs`, `enumitem`, `tcolorbox`). No
`--shell-escape`, no external assets, no `.bib` file.

## Caveats

The tutorial was written without an existing WSL2 install on the author's
machine — every command is based on stable Ubuntu 24.04 / bash 5.x surface
that has been unchanged for years, but if you hit a stale apt repo URL or
a renamed package name, please file an issue or open a PR.

## See also

A companion **PowerShell** tutorial (same audience, same style, ~25 pages)
lives at `..\powershell\`. It's useful as a secondary reference for the
times you do need to do something on the Windows side — but for daily
work, bash on WSL2 is the recommended primary shell.
