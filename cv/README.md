# CV source files

This folder stores the LaTeX source for the CV files used on the website.

## Source files

- `cv_website.tex`: website/public-facing CV source
- `cv_main.tex`: detailed/full CV source
- `biblio_cv.bib`: shared bibliography entries

## Published PDF targets

- `cv_website.tex` -> `/static/pdf/ke_lyu_cv.pdf`
- `cv_main.tex` -> `/static/pdf/main_cv.pdf`

## Website links

The website currently links to `/pdf/ke_lyu_cv.pdf` from:

- `content/CV/_index.md`
- `config/_default/menus.yaml`

## Suggested update workflow

1. Edit `cv_website.tex` or `cv_main.tex`.
2. Recompile the corresponding PDF.
3. Replace the matching file in `static/pdf/`.
4. Commit and push the repo.
