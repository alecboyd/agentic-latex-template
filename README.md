# Agentic LaTeX Template

This repository provides a structured LaTeX writing template for theorem-driven mathematical documents. It combines a reusable chapter skeleton, custom theorem/proof styling, modular TikZ figure components, and an integrated TODO system for drafting workflows.

## What You Get

- A complete article scaffold with standing notation and assumptions, local lemmas/propositions/corollaries, a main theorem section, and appendix + notation glossary patterns
- [`paper/style.sty`](paper/style.sty): theorem environments, `cleveref` setup, hyperlink styling, abstract formatting
- [`paper/extra.sty`](paper/extra.sty): TikZ helpers, math utility commands, TODO/list-of-todos system
- Reusable figure architecture via wrappers in `paper/figures/examples/` and drawing primitives in `paper/elements/examples/`
- Bibliography setup with `biblatex` (`backend=bibtex`) using [`paper/refs.bib`](paper/refs.bib)
- AI-oriented authoring guide in [`style/latex_style_guide.txt`](style/latex_style_guide.txt)

## Repository Layout

```text
.
|-- main.tex                  # Root compile entrypoint (targets content in paper/)
|-- paper/
|   |-- main.tex              # Standalone compile entrypoint inside paper/
|   |-- style.sty
|   |-- extra.sty
|   |-- refs.bib
|   |-- chapters/
|   |   `-- examples/
|   |       |-- core_template.tex
|   |       `-- appendix.tex
|   |-- figures/
|   |   `-- examples/         # Figure wrappers used by chapters
|   `-- elements/
|       `-- examples/         # Reusable TikZ drawing building blocks
|-- prompts/
|   `-- prompt-readme-gen.txt # Prompt asset for README generation workflows
`-- style/
    |-- latex_style_guide.txt
    `-- references_style.tex
```

`src/` and `experiments/` are present as directories but currently contain no files. They are intended to be used for heuristic programmed experiments, so that gathered data and code can exist within the same context window as the paper itself. 

## Requirements

- A LaTeX distribution with `pdflatex`, `bibtex`, and `latexmk`
- MiKTeX or TeX Live full installs are recommended
- No Python/Node/Cargo dependency setup is required for document compilation

## Build

### Option A (recommended): build from repository root

```powershell
latexmk -pdf -interaction=nonstopmode -file-line-error main.tex
```

Output: `main.pdf` at repository root.

### Option B: build from `paper/`

```powershell
cd paper
latexmk -pdf -interaction=nonstopmode -file-line-error main.tex
```

Output: `paper/main.pdf`.

## Authoring Workflow

1. Edit chapter content in [`paper/chapters/examples/core_template.tex`](paper/chapters/examples/core_template.tex) and [`paper/chapters/examples/appendix.tex`](paper/chapters/examples/appendix.tex).
2. Update bibliography entries in [`paper/refs.bib`](paper/refs.bib).
3. Add or modify figures, keeping chapter-facing wrappers in `paper/figures/examples/` and reusable diagram fragments in `paper/elements/examples/`.
4. Rebuild with `latexmk`.

## Agentic Workspace

This repository is designed to be used as an agentic workspace.

- The [`prompts/`](prompts/) folder contains prompt-engineered task prompts used to drive model behavior for specific repository workflows.
- Prompts reference files in the [`style/`](style/) folder when relevant, so model outputs can follow project-specific writing and formatting conventions.
- Use [`style/latex_style_guide.txt`](style/latex_style_guide.txt) as the primary conventions reference for AI and human edits inside `paper/`.
- These style files are intentionally user-modifiable and are expected to evolve as your preferences and conventions become more defined.

## TODO System

The template includes a margin-note TODO mechanism and a generated TODO list.

- Add TODOs with `\todo{Replace placeholder argument with domain-specific theorem.}` or `\todo[Short task caption]`
- Print collected TODOs with `\listoftodos`

Implementation lives in [`paper/extra.sty`](paper/extra.sty).

## Bibliography and References

- Citation package: `biblatex` with numeric style
- Backend: `bibtex`
- Bibliography file: [`paper/refs.bib`](paper/refs.bib)
- Cross-references use `cleveref` and custom theorem/equation label formatting from [`paper/style.sty`](paper/style.sty)

## Cleaning Build Artifacts

Run clean from the same directory as the `main.tex` you compiled:

```powershell
latexmk -C main.tex
```

If you compile both root and `paper/main.tex`, run clean in both locations.

## Notes

- Generated LaTeX artifacts are partially covered by [`.gitignore`](.gitignore).
- No CI pipeline, container configuration, or deployment manifests are currently defined in this repository.
- No `LICENSE` file is currently present.
