# EDS 232 Course Website — Development Spec

## File Structure

Each lesson lives in `notes/<topic>/` and contains:
- `*-NOTES.qmd` — detailed study notes rendered as HTML
- `*-SLIDES.qmd` — reveal.js slide deck (optional, not every lesson has one)
- `images/` — all figures referenced in that lesson

The `_quarto.yml` sidebar must be updated to include any new notes file.

---

## Slides Front Matter

Every `*-SLIDES.qmd` must have this YAML block (no relying on `_quarto.yml` defaults for revealjs):

```yaml
---
format:
    revealjs:
        theme: <relative-path>/styles/meds-slides-styles.scss
        slide-number: true
        chalkboard: true
        title-slide: false
jupyter: eds232-env
---
```

The theme path is relative to the file's location (e.g., `../../styles/...` for two levels deep, `../../../styles/...` for three).

---

## Slide Layout Patterns

### Title / cover slide
```markdown
## {background="#047C90"}

[Lesson Title]{.custom-title}

[Subtitle]{.custom-subtitle}
```

### Content slide
```markdown
## {#anchor data-menu-title="Short nav title"}

[Full Slide Title]{.slide-title}

<hr>
```
The `<hr>` renders as a dark-blue horizontal rule (from the scss). Do not use `---` inside a slide; use `---` only between slides.

### Section break (teal background)
```markdown
## {background="#047C90"}

[Section Name]{.custom-title}
```

### Check-in: plot or diagram question
Use a two-column layout so question and answer sit side-by-side (answer revealed with `. . .`):

```markdown
:::: {.columns}
::: {.column width="55%"}
**Question text or plot**
:::
::: {.column width="45%"}
. . .

**Answer**
:::
::::
```

### Check-in: text-only question
Use `. . .` on its own line to reveal the answer:

```markdown
**Q:** question text?

. . .

**A:** answer text.
```

---

## CSS Classes (from `meds-slides-styles.scss`)

| Class | Use |
|-------|-----|
| `.custom-title` | Cover slide main title (serif, baby-blue, 3em) |
| `.custom-subtitle` | Cover slide subtitle (sans-serif, baby-blue, 2.4em) |
| `.custom-subtitle2` | Cover slide subtitle medium (1.8em) |
| `.custom-subtitle3` | Cover slide subtitle small (1.2em) |
| `.slide-title` | Content slide title (teal, 2em, bold) |
| `.slide-title2` | Content slide title medium (1.7em) |
| `.slide-title3` | Content slide title small (1.5em) |
| `.body-text-m` | Body text enlarged (1.3em) |
| `.body-text-s` | Body text small (20px) — use sparingly |
| `.teal-text` | Teal color `#047C90` |
| `.dark-blue-text` | Dark blue `#003660` |
| `.baby-blue-text` | Baby blue `#d2e3f3` |

**Do not apply `.body-text-s` to hyperparameter or comparison tables** — tables already have a defined font size from the scss (`th` = 30px, `td` = 20px). Wrapping tables in `.body-text-s` makes them too small.

---

## Brand Colors

| Name | Hex |
|------|-----|
| Teal | `#047C90` |
| Dark blue | `#003660` |
| Baby blue | `#d2e3f3` |
| Green | `#78A540` |
| Magenta | `#900C3F` |

---

## Python / Code Conventions

- **Kernel:** `eds232-env` (Conda, Python 3.10)
- **Every code chunk must have `#| label:`** — required for Quarto cross-references and freeze cache management.
- **Private variables:** prefix with `_` (e.g., `_X_tr`, `_acc_lr`) to avoid polluting the notebook namespace across chunks.
- **Plot style:** `plt.style.use('default')`, `fig_size_x = 8`, `fig_size_y = 4`, `plt.rcParams['font.size'] = 11`
- **Color maps:** use `steelblue` / `tomato` (or `col_map` dict) consistently for two-class problems.

---

## Exercise Design Conventions


### Standardization
- Always apply `StandardScaler` before distance- or kernel-based models (KNN, SVM). State this requirement in the exercise setup.

### Scenario design
- Use **environmental / ecological settings** grounded in real UCSB MEDS/MESM contexts (kelp, fire, species distribution, etc.).
- Scenarios must have a plausible real-world prediction use case (e.g., pre-screening sites for fuel management intervention).
- Variable relationships should have a natural ecological interpretation — avoid purely contrived geometric datasets (e.g., XOR) without ecological motivation.

---

## Notes Formatting

- Center all plots with `::: {style="text-align: center;"}` or equivalent Quarto div.
- Use callout blocks (`::: {.callout-note}`, `.callout-tip`, `.callout-warning`) for key definitions and caveats.
- Exercises go at the end of notes files under a `## Exercises` heading with answers in a collapsible callout or answer block.
- Include answers in the notes (this is a graduate-level course; answers are part of the learning material).

---

## Rendering

```bash
quarto render                          # full site → docs/
quarto render notes/path/to/file.qmd   # single file
quarto preview                         # live preview
```

`execute: freeze: auto` means code only re-runs when the source file changes. If a chunk needs forced re-execution, delete the corresponding `_freeze/` entry.
