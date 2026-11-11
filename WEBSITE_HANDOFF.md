# Website Handoff Guide

This note explains how to maintain and publish the website without needing to remember the project structure.

## Where Everything Is Stored

### Website repository

- **Local website folder:** `/Users/air/Developer/KerrLyu.github.io`
- **Live website:** `https://kerrlyu.github.io/`
- **GitHub repository:** `https://github.com/KerrLyu/KerrLyu.github.io`

This folder is the source of truth for the public website. Open it in VS Code, another text editor, or GitHub Desktop's **Show in Finder** button.

The folder is deliberately kept **outside** `Documents`. `Documents` is synced to iCloud with "Optimize Mac Storage" turned on, and when the disk gets full macOS offloads files to the cloud, leaving empty placeholders. That broke the Hugo build (it hung while processing CSS) and also offloaded files inside `.git`. Do not move this folder back into `Documents` or `Desktop`, and when GitHub Desktop clones a new repository, choose a location outside those two folders too.

### Main website content

| What you want to change | File or folder |
| --- | --- |
| Homepage profile, address, social links, self-introduction | `content/homepage/about.md` |
| Research page, publications, working papers | `content/research/_index.md` |
| Teaching page, courses, and game cards | `content/teaching/_index.md` |
| Individual game pages | `content/teaching/` |
| Public CV page | `content/CV/_index.md` |
| Logo | `assets/images/logo.png` |
| Homepage portrait (480 px, also the link-preview image) | `static/jpg/portrait_480.jpg` |
| Website colors and appearance | `static/css/custom.css` and `data/styles/ink-and-sage.yaml` |
| Navigation menu | `config/_default/menus.yaml` |
| Site-wide settings, including the generated output folder | `config/_default/config.yaml` |
| GoatCounter settings | `config/_default/params.yaml` |

### Teaching game files

All downloadable game materials are under `static/teaching-games/`.

- Paper Plane Factory: `static/teaching-games/econglider/`
- Contribution Game: `static/teaching-games/contribution/`
- Market Game: `static/teaching-games/market/`
- Monopoly / The Price Is Yours: `static/teaching-games/monopoly/`
- Coffee Shop Cost Builder: `static/teaching-games/coffee-shop/`

Files placed in `static/` appear on the website at the same path without the `static` portion. For example, `static/teaching-games/market/Market_Game_Cards.pdf` becomes `/teaching-games/market/Market_Game_Cards.pdf` on the live site.

### CV files

There are two separate CV versions. Keep them separate.

| Version | Source location | Published or output PDF |
| --- | --- | --- |
| Public website CV | `cv/cv_website.tex` and `cv/biblio_cv.bib` in this repository | `static/pdf/ke_lyu_cv.pdf` |
| Detailed public CV | `cv/cv_main.tex` and `cv/biblio_cv.bib` in this repository | `static/pdf/main_cv.pdf` |
| Private CV with submission statuses | `/Users/air/Documents/New project/cv_private_2026/cv_website.tex` and `biblio_cv.bib` | `/Users/air/Desktop/Ke_Lyu_CV_Private_2026.pdf` |

The private CV is not uploaded to the website. The public CV should not include confidential submission or revise-and-resubmit statuses unless you intentionally decide to add them.

## Safest Publishing Workflow

1. Edit the source file you need in `/Users/air/Developer/KerrLyu.github.io`.
2. Open Terminal and run the following from the website folder:

   ```bash
   hugo --noBuildLock
   ```

   This rebuilds the public website into `docs/`.

   To preview locally before publishing, use `hugo server --renderToMemory` and open `http://localhost:1313/`. Without `--renderToMemory`, the preview server writes `localhost` links into `docs/`, and committing those would break the live site.
3. Open GitHub Desktop. You should see both the source-file changes and changes inside `docs/`.
4. Check that `docs/` is included, write a short commit summary, then click **Commit to master**.
5. Click **Push origin**.
6. Wait a few minutes, then refresh `https://kerrlyu.github.io/` in a private/incognito browser window.

### Important publishing rule

Do not edit files inside `docs/` by hand. Hugo recreates that folder every time it builds the site. Instead, edit the source files under `content/`, `static/`, `assets/`, `layouts/`, `config/`, or `data/`, run Hugo, and commit the resulting `docs/` changes too.

## Small Edits You Can Make Safely

### Change the homepage text, address or social links

Open `content/homepage/about.md`. Each social link has an `icon`, a `label` (what screen readers say and what shows on hover) and a `url`.

### Change the portrait

Resize the new photo to a 480 × 480 square first, for example `sips -Z 480 new.jpg --out static/jpg/portrait_480.jpg`. A full-size photo makes the homepage several times slower to load on phones.

### Update publications or working papers

Open `content/research/_index.md`. Each entry is an HTML block. The Publications and Working Papers sections are public; the Work in Progress section is currently intentionally hidden inside an HTML comment.

### Update teaching courses

Open `content/teaching/_index.md`. Course history and the classroom-game cards are both maintained there.

### Add a new game file

1. Create a clear subfolder under `static/teaching-games/`, such as `static/teaching-games/new-game/`.
2. Put the instructor guide, student materials, slides, and any images in that folder.
3. Create a matching page under `content/teaching/`.
4. Add a game card to `content/teaching/_index.md`.
5. Run Hugo and publish using the workflow above.

## Updating a Public CV

1. Edit `cv/cv_website.tex` or `cv/cv_main.tex` and, if needed, `cv/biblio_cv.bib`.
2. Compile the corresponding `.tex` file to PDF.
3. Replace the matching PDF in `static/pdf/`.
4. Run `hugo --noBuildLock`, then commit and push both source and `docs/` changes.

The public CV page links to `/pdf/ke_lyu_cv.pdf`.

## If Something Looks Wrong

- **The live website did not change:** Confirm you ran Hugo and committed the generated `docs/` folder, then wait a few minutes and force-refresh the browser.
- **A file link returns 404:** Check that the file is inside `static/` and that the page link omits `static/`.
- **Formatting suddenly looks unstyled:** Make sure `static/css/custom.css` still exists and that `config/_default/config.yaml` lists it under `params.customCSS`.
- **`hugo` hangs for minutes, or fails with `partial "head" timed out after 30s`:** Some files in the folder have probably been offloaded to iCloud. Check that the folder is still at `/Users/air/Developer/KerrLyu.github.io` and not inside `Documents` or `Desktop`. To find offloaded files, run `find . -type f -flags +dataless | wc -l` from the website folder; anything above 0 means files need to be downloaded again (`brctl download <path>`).
- **GitHub Desktop says the repository is missing:** Click **Locate…** and choose `/Users/air/Developer/KerrLyu.github.io`.
- **GitHub Desktop is stuck refreshing:** Restart GitHub Desktop. If a commit is shown as waiting to be pushed, use the **Push origin** button after the refresh completes.

- **A page needs math formulas, code blocks or diagrams:** These are switched off in `config/_default/params.yaml` because no page used them and they made every page much slower. Set `math.handler: katex`, `highlight.handler: highlightjs` or `diagram.handler: mermaid` to switch one back on.
- **Tag, category, series or author pages are missing:** They are switched off on purpose (`disableKinds` in `config/_default/config.yaml`), because the site has none and the empty pages showed up in Google.

## Before Making Larger Changes

Make a quick backup by copying the whole repository folder or creating a Git commit first. For major layout or color changes, edit one section at a time, run Hugo, and check the homepage, Research, Teaching, and mobile layout before pushing.
