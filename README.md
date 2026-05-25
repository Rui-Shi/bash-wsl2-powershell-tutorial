# Shell Tutorials for the Data & ML Engineer

Two self-contained LaTeX tutorials — one for **Bash on WSL2** and one for
**PowerShell on Windows** — aimed at Python / cron / AWS / Mongo / MySQL
users who need to be productive in a shell on whatever machine they sit
in front of. Each is a single `.tex` file plus the rendered PDF, with
worked examples that use realistic data (CSV rows, JSON API responses,
log files) rather than `foo` / `bar`.

## Which one should I read?

| Your situation | Read this |
|---|---|
| You work on a Windows laptop and your production code runs on Linux (EC2, Docker, K8s, CI) | **`Bash-WSL2-Tutorial.pdf`** — primary, daily driver |
| You occasionally need to do something on the Windows side (file ops, env-var setup, Azure scripting) | **`PowerShell-Tutorial.pdf`** — secondary reference |
| You're new to both | Start with the Bash tutorial; it includes the WSL2 setup steps and assumes nothing |

The short version: **bash is what runs in production, so it's the better
long-term investment**. PowerShell is useful for the things you'll keep
doing on Windows (the rest of your laptop), and the object-pipeline
mental model it teaches generalizes to `jq` and `pandas` later.

## What's in each tutorial

### Bash on WSL2 (~36 pages, 20 sections in 7 parts)

- **Part I — WSL2 Setup & Mental Model.** `wsl --install -d Ubuntu`, the
  Linux filesystem, why your code lives in `~/projects` not `/mnt/c/`,
  VS Code Remote-WSL.
- **Part II — Bash Fundamentals.** Navigation, globbing, pipes,
  stdin/stdout/stderr, variables and quoting, parameter expansion, then
  the `grep` / `sed` / `awk` / `sort` / `uniq` toolkit with a worked
  "top 5 IPs in a log" pipeline.
- **Part III — Data Wrangling.** CSV with `awk`, `mlr` (miller), and
  `csvkit`; JSON with `jq`; HTTP / paginated APIs with `curl + jq`.
- **Part IV — Environment & Python.** `export`, `.bashrc`, the `.env`
  secrets pattern, Python on Ubuntu, `uv` for venv and package
  management.
- **Part V — Data / ML Ecosystem.** Docker on WSL2 (Docker Desktop +
  `docker compose`), CUDA / GPU passthrough for PyTorch, AWS CLI v2,
  `mongosh`, MySQL client.
- **Part VI — Scripting & a Real ETL.** `set -euo pipefail` discipline,
  conditionals, loops, `trap`, CLI parsing, then a complete `enrich.sh`
  that ties CSV input + `curl` enrichment + `jq` + `uv run` Python +
  JSON output into one script.
- **Part VII — Reference.** Footgun checklist plus a 3-column lookup
  table (Python concept ↔ Bash ↔ PowerShell).

### PowerShell on Windows (~25 pages, 15 sections in 5 groups)

- **Fundamentals.** Why PowerShell pipes *objects* (not bytes), the
  Verb-Noun cmdlet convention, navigation, the object pipeline
  (`Where-Object`, `Select-Object`, `ForEach-Object`, `Sort-Object`,
  `Group-Object`, `Measure-Object`), variables and hashtables, strings
  and here-strings, file I/O, encoding.
- **Data Wrangling.** CSV with `Import-Csv` / `Export-Csv` (including a
  hashtable-based join), JSON with `ConvertFrom-Json` /
  `ConvertTo-Json`, REST APIs with retries / pagination via
  `Invoke-RestMethod`, parallel ingestion with `ForEach-Object
  -Parallel`.
- **Python Workflows.** `$env:` for AWS / Mongo / MySQL credentials,
  the `py` launcher, `venv` activation (and the one-time
  `Set-ExecutionPolicy` fix), `uv` on PowerShell.
- **A Real ETL.** Mini end-to-end walkthrough mirroring the bash version.
- **Reference.** Pitfalls and a `bash → PowerShell` translation table.

## Files

| File | What it is |
|---|---|
| `Bash-WSL2-Tutorial.tex` | LaTeX source (~64 KB, self-contained) |
| `Bash-WSL2-Tutorial.pdf` | Rendered output (~36 pages) |
| `PowerShell-Tutorial.tex` | LaTeX source (~50 KB, self-contained) |
| `PowerShell-Tutorial.pdf` | Rendered output (~25 pages) |
| `*.aux` / `*.log` / `*.out` / `*.toc` | Standard `pdflatex` auxiliary files (regenerated on build; safe to delete or `.gitignore`) |

## Build

Both tutorials use the same toolchain — any TeX Live or MiKTeX install
works. Two passes per file for the table of contents to resolve:

```powershell
# Bash tutorial
pdflatex -interaction=nonstopmode Bash-WSL2-Tutorial.tex
pdflatex -interaction=nonstopmode Bash-WSL2-Tutorial.tex

# PowerShell tutorial
pdflatex -interaction=nonstopmode PowerShell-Tutorial.tex
pdflatex -interaction=nonstopmode PowerShell-Tutorial.tex
```

Required LaTeX packages (all standard, all bundled with TeX Live and
MiKTeX): `listings`, `xcolor`, `hyperref`, `geometry`, `setspace`,
`parskip`, `booktabs`, `array`, `enumitem`, `tcolorbox`. No
`--shell-escape`, no external figures, no `.bib` file.

## Suggested `.gitignore`

The aux files are regenerated on every build — don't commit them:

```gitignore
*.aux
*.log
*.out
*.toc
*.synctex.gz
```

## Caveats

The bash tutorial was written without a live WSL2 install on the
author's machine — every command targets stable Ubuntu 24.04 / bash 5.x
surface that has been unchanged for years, but if you hit a renamed apt
package or stale repo URL, please open an issue or PR.

## License

Free to read, fork, and adapt. If you redistribute meaningful chunks of
the prose, a credit link back to this repo is appreciated.
