# AgriConnect — PFE Report

LaTeX source for the end-of-studies report on **AgriConnect**, an intelligent agricultural assistance platform for Moroccan farmers.

---

## Prerequisites

Install one of these LaTeX distributions:

| OS | Distribution | Download |
|----|-------------|---------|
| Windows | MiKTeX (recommended) | https://miktex.org |
| Windows / Linux / macOS | TeX Live | https://tug.org/texlive |
| macOS | MacTeX | https://www.tug.org/mactex |

**Required tool: Biber** (bibliography processor — included in MiKTeX and TeX Live).

Optional but recommended: [VS Code](https://code.visualstudio.com) + [LaTeX Workshop extension](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop).

---

## Compiling the Report

### Option 1 — latexmk (recommended, one command)

```bash
latexmk -pdf -interaction=nonstopmode main.tex
```

This handles all passes (pdflatex + biber + pdflatex × 2) automatically.
The output is `main.pdf`.

To clean build artifacts:

```bash
latexmk -C
```

### Option 2 — Manual step-by-step

```bash
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex
```

Run `pdflatex` three times to resolve cross-references and the table of contents.

### Option 3 — VS Code with LaTeX Workshop

1. Open the repo folder in VS Code.
2. Install the **LaTeX Workshop** extension.
3. Open `main.tex` and press `Ctrl+Alt+B` to build.
4. Press `Ctrl+Alt+V` to open the PDF preview side by side.

LaTeX Workshop uses `latexmk` by default, so no extra configuration is needed.

---

## Repository Structure

```
pfe-report/
├── main.tex                  # Entry point — do not edit chapters here
├── bibliography/
│   └── references.bib        # All BibTeX references
├── chapters/
│   ├── introduction.tex      # Chapter 1 — Introduction
│   ├── etat_art.tex          # Chapter 2 — State of the Art
│   ├── analyse.tex           # Chapter 3 — Requirements Analysis
│   ├── conception.tex        # Chapter 4 — System Design
│   ├── realisation.tex       # Chapter 5 — Implementation
│   ├── resultats.tex         # Chapter 6 — Results & Evaluation
│   └── conclusion.tex        # Chapter 7 — Conclusion
├── frontmatter/
│   ├── cover.tex             # Title page
│   ├── abstract.tex          # Abstract
│   └── acknowledgements.tex  # Acknowledgements
├── images/
│   └── diagrammes/           # UML diagrams and figures
└── annexes/
    └── annexeA.tex           # Appendix A
```

---

## Writing Guide for Both Authors

### Golden rule: one file per chapter

Each chapter lives in its own `.tex` file under `chapters/`. **Never write directly in `main.tex`** — it only assembles the pieces.

### Heading hierarchy

Inside every chapter file, use this structure:

```latex
\chapter{Chapter Title}       % top level — one per file

\section{Section Title}       % main sections

\subsection{Subsection}       % sub-sections if needed
```

### Adding a figure

Place the image file in `images/` (or `images/diagrammes/` for UML), then:

```latex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.8\textwidth]{images/diagrammes/my_diagram.png}
  \caption{Description of the figure.}
  \label{fig:my-diagram}
\end{figure}
```

Reference it in the text with `\ref{fig:my-diagram}`.

### Adding a citation

Add the entry to `bibliography/references.bib`:

```bibtex
@article{author2025topic,
  author  = {Author, First and Author, Second},
  title   = {Title of the Paper},
  journal = {Journal Name},
  year    = {2025},
  volume  = {10},
  pages   = {1--20},
  doi     = {10.xxxx/xxxxx}
}
```

Then cite it in the text: `\cite{author2025topic}`.

---

## Git Workflow (Collaboration)

Each author works on their own chapters to avoid conflicts.

### Suggested chapter split

| Author | Chapters |
|--------|---------|
| Author 1 | Introduction, State of the Art, Requirements Analysis |
| Author 2 | System Design, Implementation, Results, Conclusion |

### Daily workflow

```bash
# Before starting — always pull first
git pull origin main

# After writing — add only your chapter files
git add chapters/conception.tex
git commit -m "Add architecture section to System Design chapter"
git push origin main
```

### If you get a merge conflict

Conflicts only happen when both authors edited the **same file** at the same time.
Open the conflicted file, look for the markers, and keep the correct version:

```
<<<<<<< HEAD
  your version
=======
  their version
>>>>>>> origin/main
```

Delete the markers and the unwanted version, then:

```bash
git add chapters/the_conflicted_file.tex
git commit
git push origin main
```

---

## Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `undefined citations` | Biber not run | Run `biber main` then `pdflatex main` again |
| `undefined references` | Only one pdflatex pass | Run `pdflatex` at least twice after biber |
| `File not found` | Wrong path in `\includegraphics` | Check the path relative to `main.tex` |
| `Package not found` | Missing LaTeX package | Open MiKTeX Console → Updates, or run `tlmgr install <package>` |
