# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Quarto-based course website for **EDS 232 - Machine Learning for Environmental Science** (UCSB MEDS program). It generates both a navigable website and reveal.js slide presentations from `.qmd` source files, published to GitHub Pages via the `docs/` directory.

## Build Commands

```bash
# Render the full site (outputs to docs/)
quarto render

# Live preview with hot reload
quarto preview

# Render a single file
quarto render notes/path/to/file.qmd
```

## Architecture

**Source → Output flow:**
- Source `.qmd` files live in `notes/` (organized by lesson topic)
- Quarto renders them into `docs/` (served by GitHub Pages)
- `execute: freeze: auto` in `_quarto.yml` caches code execution — only re-renders on source changes

**Content organization:**
- Each lesson folder under `notes/` contains a `*-NOTES.qmd` (detailed study notes) and optionally a `*-SLIDES.qmd` (reveal.js presentation)
- Images for each lesson are in a sibling `images/` subdirectory
- `references/references.bib` holds shared bibliography entries

**Styling:**
- Website theme: Quarto "cosmo" + custom "brand" theme
- Slide theme: `styles/meds-slides-styles.scss` — defines custom CSS classes used extensively in slides (`.slide-title`, `.body-text-m`, `.body-text-s`, etc.) and color variables (teal `#047C90`, dark blue `#003660`, etc.)
- Additional site CSS: `styles.css`

**Slide-specific front matter:** Slides `.qmd` files must include the revealjs format block directly in their YAML front matter (not just relying on `_quarto.yml` defaults) for proper rendering.

## Python Environment

The course uses a Conda environment with Python 3.10:

```bash
conda env create --name eds232-env --file eds232-env.yml
conda activate eds232-env
python -m ipykernel install --user --name eds232-env --display-name "eds232-env"
```

Key packages: scikit-learn, xgboost, tensorflow, keras, torch, pandas, numpy, matplotlib, seaborn, otter-grader.
