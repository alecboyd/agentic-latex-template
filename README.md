# Agentic LaTeX Template

## What This Repository Is

This repository provides a structured LaTeX writing template for theorem-driven mathematical documents. It combines a reusable chapter skeleton, custom theorem/proof styling, modular TikZ figure components, and an integrated TODO system for drafting workflows.

## Core Features

- A complete article scaffold with standing notation and assumptions, local lemmas/propositions/corollaries, a main theorem section, and appendix + notation glossary patterns
- Two compile entrypoints: [`main.tex`](main.tex) (root build) and [`paper/main.tex`](paper/main.tex) (in-folder build)
- [`paper/style.sty`](paper/style.sty): theorem environments, `cleveref` setup, hyperlink styling, abstract formatting
- [`paper/extra.sty`](paper/extra.sty): TikZ helpers, math utility commands, TODO/list-of-todos system
- Reusable figure architecture via wrappers in `paper/figures/examples/` and drawing primitives in `paper/elements/examples/`
- Bibliography setup with `biblatex` (`backend=bibtex`) using [`paper/refs.bib`](paper/refs.bib)
- Report output workspace at [`paper/reports/`](paper/reports/) for proof/conjecture task prompts
- AI-oriented authoring guide in [`style/latex_style_guide.txt`](style/latex_style_guide.txt)
- Prompt assets for repository workflows in [`prompts/`](prompts/)
- Repository state ledgers at [`state/current-state.txt`](state/current-state.txt) and [`state/changelog.txt`](state/changelog.txt)

## Repository Layout

```text
.
|-- main.tex                  # Root compile entrypoint (targets content in paper/)
|-- README.md
|-- paper/
|   |-- main.tex              # Standalone compile entrypoint inside paper/
|   |-- style.sty
|   |-- extra.sty
|   |-- refs.bib
|   |-- reports/
|   |   `-- .gitkeep
|   |-- chapters/
|   |   `-- examples/
|   |       |-- core_template.tex
|   |       `-- appendix.tex
|   |-- figures/
|   |   `-- examples/         # Figure wrappers used by chapters
|   `-- elements/
|       `-- examples/         # Reusable TikZ drawing building blocks
|-- prompts/
|   |-- 0 - Meta/
|   |-- 1 - Transfer and Maintenence/
|   |-- 2 - Paper Tasks/
|   `-- 3 - Mathematical Tasks/
|-- state/
|   |-- current-state.txt
|   `-- changelog.txt
`-- style/
    `-- latex_style_guide.txt
```

`src/`, `experiments/`, and `merge/` are active working directories (tracked with `.gitkeep` until you add project content). `merge/` is the staging area for automated LaTeX manuscript transfer.

## Requirements

- A LaTeX distribution with `pdflatex`, `bibtex`, and `latexmk`
- MiKTeX or TeX Live full installs are recommended
- No Python/Node/Cargo dependency setup is required for document compilation

## Build / Compile

### Option A (recommended): build from `paper/`

```powershell
cd paper
latexmk -pdf -interaction=nonstopmode -file-line-error main.tex
```

Output: `paper/main.pdf`.

### Option B: build from repository root

```powershell
latexmk -pdf -interaction=nonstopmode -file-line-error main.tex
```

Output: `main.pdf` at repository root.
If bibliography content changed, run Option A at least once so `refs.bib` is refreshed through the in-folder build flow.

### Troubleshooting stale build state

If you switch between root and `paper/` builds and hit aux/bibliography path errors, clean and rebuild:

```powershell
latexmk -C main.tex
```

Run it once at repository root and once inside `paper/`, then compile again.

## Authoring Workflow

1. Treat `paper/*/examples/` as reference templates. Create manuscript files in non-`examples/` paths such as `paper/chapters/`, `paper/figures/`, and `paper/elements/`.
2. Copy from [`paper/chapters/examples/core_template.tex`](paper/chapters/examples/core_template.tex) and [`paper/chapters/examples/appendix.tex`](paper/chapters/examples/appendix.tex) into your active chapter files.
3. Update includes:
   - In [`main.tex`](main.tex): use `\include{paper/chapters/<your-file>}`
   - In [`paper/main.tex`](paper/main.tex): use `\include{chapters/<your-file>}`
4. Add or modify figures using wrapper files in `paper/figures/` and reusable TikZ primitives in `paper/elements/`.
5. Update bibliography entries in [`paper/refs.bib`](paper/refs.bib), then rebuild with `latexmk`.

## Style and Prompt System

This repository is designed to be used as an agentic workspace.

- Prompt assets are grouped by workflow family under [`prompts/`](prompts/):
  - [`prompts/0 - Meta/`](prompts/0%20-%20Meta/)
  - [`prompts/1 - Transfer and Maintenence/`](prompts/1%20-%20Transfer%20and%20Maintenence/)
  - [`prompts/2 - Paper Tasks/`](prompts/2%20-%20Paper%20Tasks/)
  - [`prompts/3 - Mathematical Tasks/`](prompts/3%20-%20Mathematical%20Tasks/)
- Use [`style/latex_style_guide.txt`](style/latex_style_guide.txt) as the primary conventions reference for AI and human edits inside `paper/`.
- `style/references_style_guide.txt` is not present in the current repository state.
- Use [`state/current-state.txt`](state/current-state.txt) and [`state/changelog.txt`](state/changelog.txt) when maintaining repository state snapshots.
- Style files are user-modifiable and intended to evolve with your authoring preferences.

## LaTeX Conventions

By default, keep files inside any `examples/` subfolder unchanged and use them as templates.

### TODO System

The template includes a margin-note TODO mechanism and a generated TODO list.

- Add TODOs with `\todo{Replace placeholder argument with domain-specific theorem.}` or `\todo[Short task caption]`
- Print collected TODOs with `\listoftodos`
- For inline TODOs in sentence text, keep `\todo` directly attached to the previous word (no inserted space) to avoid output spacing artifacts.

Implementation lives in [`paper/extra.sty`](paper/extra.sty).

### Cross-Reference Conventions

- Use `\cref{...}` for equation/theorem-style references.
- Label prefixes used by the template and style guide: `sec:`, `ssec:`, `sssec:`, `eq:`, `def:`, `rem:`, `lem:`, `prop:`, `cor:`, `thm:`, `ex:`, `app:`.

## Bibliography and References

- Citation package: `biblatex` with numeric style
- Backend: `bibtex`
- Bibliography file: [`paper/refs.bib`](paper/refs.bib)
- Cross-references use `cleveref` and custom theorem/equation label formatting from [`paper/style.sty`](paper/style.sty)

## Start-up Guide

To move from a plain LaTeX `.zip` project and source code / experiment results into this agentic template:

1. Unpack your source project into `merge/`.
2. Run [`prompts/1 - Transfer and Maintenence/prompt_merge_tex.txt`](prompts/1%20-%20Transfer%20and%20Maintenence/prompt_merge_tex.txt) with your AI agent. No extra inputs are required.
3. After the merge run reports success, validate the transfer result in `paper/` manually (content placement and compile status).
4. Run [`prompts/2 - Paper Tasks/prompt_detail_level_evaluator.txt`](prompts/2%20-%20Paper%20Tasks/prompt_detail_level_evaluator.txt) to calibrate detail-level policy for the transferred manuscript.
5. Manually copy any source code into `src/` and any experiment results into `experiments/`. It is useful to keep the directories as is because future prompts will use them, and by default keep experiment results in CSV form. 
6. Run [`prompts/1 - Transfer and Maintenence/prompt_readme_update.txt`](prompts/1%20-%20Transfer%20and%20Maintenence/prompt_readme_update.txt) and [`prompts/1 - Transfer and Maintenence/prompt_state_update.txt`](prompts/1%20-%20Transfer%20and%20Maintenence/prompt_state_update.txt). In some cases, it is useful to test a pass where `README.md` and `State/`content are cleared first so the agent anchors more strongly to user manuscript content.

## Cleaning Build Artifacts

Run clean from the same directory as the `main.tex` you compiled:

```powershell
latexmk -C main.tex
```

If you compile both root and `paper/main.tex`, run clean in both locations.

## Notes / Limitations

- Generated LaTeX artifacts are partially covered by [`.gitignore`](.gitignore).
- No CI pipeline, container configuration, or deployment manifests are currently defined in this repository.
- No `LICENSE` file is currently present.
