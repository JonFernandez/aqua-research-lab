# Maintaining the AQuA Research Lab website

This guide is for whoever looks after the site. You do not need to be a web developer. If you can edit a text file carefully and use GitHub in a browser, you can maintain it.

## 1. What is in this folder

```
index.html                         Homepage
approach.html                      How we work, quantum computing vs quantum probability, responsible use
work-with-us.html                  Short invitation, contact buttons, four pathways
collaboration-guide.html           Secondary page: authorship, data, ethics, field permissions
research/index.html                Presented research, developing proposals, status labels
research/does-measuring-a-decision-change-it.html          Poster page (QISTCon.PH 2026)
research/do-ai-respondents-show-quantum-signatures.html    Poster page (QISTCon.PH 2026)
research/kawayan-carbon-from-orbit.html
research/early-warning-from-above.html
research/island-grid-intelligence.html
404.html                           "Page not found" page
assets/css/site.css                All colours, fonts and layout
assets/img/                        Illustrations (SVG), logo mark, favicon
assets/posters/                    Poster PDFs and their JPEG thumbnails
.nojekyll                          Tells GitHub Pages to publish the files exactly as they are
README.md, MAINTAINING.md, CREDITS.md
```

There is no build step. What you see in these files is what gets published.

## 2. Preview on your computer

Double-click `index.html` to open it in a browser. For a more realistic preview, open a terminal in this folder and run:

```bash
python3 -m http.server 8000
```

Then visit http://localhost:8000. Press Ctrl+C in the terminal to stop.

## 3. Common edits

### Change some text
Open the page in a text editor (or click the file on GitHub, then the pencil icon). Find the sentence and change it. Keep the tags around it (`<p>…</p>`, `<li>…</li>`) intact.

### Change a project's status
Each project shows its status in three places. Update all three together:

1. The homepage card in `index.html`
2. The card in `research/index.html`
3. The top of the project's own page, and the "Status" line in its sidebar

Status labels look like this:

```html
<span class="status status-presented">Poster presented · QISTCon.PH 2026</span>
<span class="status status-proposed">Proposed research</span>
<span class="status status-concept">Research concept</span>
```

Use "Published" only when a paper has actually been published, and link it.

When a project is funded or under way, you can add a new label, for example `<span class="status status-proposed">Active research</span>`, and change the note under it. Only change a status once it is true and can be stated publicly.

### Add a new research project
1. Copy `research/island-grid-intelligence.html` to a new file with a short, lowercase, hyphenated name, for example `research/reef-acoustics.html`.
2. Replace the title, description, status and every section. Delete anything that does not apply. Keep the headings in order: In brief, The real-world problem, The scientific question, Proposed approach, Intended outputs, Who might benefit, Status and limitations, How you could help.
3. Add a card to `research/index.html` and, if it is a lead project, to `index.html`. Copy an existing `<article class="card project-card">…</article>` block.
4. Add it to the "More research" list at the bottom of the other project pages.

### Show detail only when a reader asks for it
The site keeps pages short by putting extra detail behind click-to-expand sections. Copy this pattern:

```html
<details class="more">
  <summary>More detail on what fell short</summary>
  <div class="more-body">
    <p>The extra detail goes here.</p>
  </div>
</details>
```

It works with mouse, touch and keyboard, and needs no JavaScript. Keep anything a reader must not miss, such as status, evidence type and key limitations, **outside** the collapsed part. Avoid hover-only content: it does not work on phones or for keyboard users.

### Add a presented poster or paper
Copy `research/does-measuring-a-decision-change-it.html`. Put the PDF and a thumbnail in `assets/posters/`, named `firstauthor-secondauthor-year-venue-short-title.pdf`. To make a thumbnail, run `pdftoppm -jpeg -scale-to 640 -singlefile poster.pdf poster-thumb`. Keep the authors in the original order and with their affiliations as printed.

### Change the contact address
The address appears twice in `work-with-us.html`: near the top and in the "Start a conversation" section. Search for `mailto:` and change both, including the visible text next to each button.

### Add people (launch step)
These are deliberately not in the site yet, because they need the lab lead's approval. When approved:

1. If a people section is approved, add it as its own section, or create `people.html` by copying `approach.html` and replacing its content. Then add it to the navigation on **every** page (see below).

Do not add a contact form unless it is connected to a service that actually delivers messages and the lab has approved that service.

### Change the navigation
The menu is copied at the top of every page (inside `<nav class="site-nav">`) and in the footer. If you add or rename a page, update **every page** (currently 10, not counting 404.html). Pages inside `research/` use `../` in front of every link and image path; pages at the top level do not.

Mark the current page with `aria-current="page"` on its menu link, so screen-reader users and the underline both show where they are.

### Images
- Put images in `assets/img/`.
- Prefer SVG for diagrams and compressed JPEG or WebP (under 300 KB) for photos.
- Always write useful `alt` text describing what the image shows. Purely decorative images get `alt=""`.
- Record the source, creator and licence or permission in `CREDITS.md` before publishing.
- Never use photos of people, communities or field sites without their permission, and never label an illustration as real data.

### Colours and fonts
All colours are at the top of `assets/css/site.css`. Every text/background pair currently passes WCAG AA contrast (the lowest is 4.95:1). If you change a colour, check it with a contrast checker such as WebAIM's.

## 4. Rules for paths (why the site works anywhere)

Every link and image uses a **relative** path, such as `approach.html` or `../assets/css/site.css`. Never start a path with `/`. This is what lets the same files work at `https://<account>.github.io/<repository>/` and at a custom domain.

## 5. Publish on GitHub Pages

You need a GitHub account with permission to create a repository for the lab. On GitHub Free, Pages publishes from **public** repositories. The website repository should therefore contain only this folder, and never the private launch materials, proposals, budgets or letters.

1. Create a new repository on GitHub (for example `aqua-research-lab`).
2. Upload the **contents** of this folder (not the folder itself) so that `index.html` sits at the top of the repository. Include the hidden `.nojekyll` file. On a Mac, press Cmd+Shift+. in Finder to show hidden files.
3. In the repository, open **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**. Select branch `main` and folder `/ (root)`, then **Save**.
5. After a minute or two the page shows the site's address, usually `https://<account>.github.io/<repository>/`.
6. Visit every page on the live site, then follow the post-launch checks in the private launch checklist.

To update the site later, edit files on GitHub (pencil icon → **Commit changes**) or upload replacements. Changes appear within a few minutes.

### Custom domain (optional)
If the lab gets a domain or subdomain, enter it under **Settings → Pages → Custom domain** and follow GitHub's DNS instructions for that domain. Your institution's IT office will usually need to add the DNS record. Tick **Enforce HTTPS** once it becomes available. No file changes are needed because all paths are relative.

## 6. Moving to Google Sites instead

Choose Google Sites if browser-based editing by several non-technical people matters more than a custom design. You would lose the custom layout, status-label styling and some accessibility control, and gain point-and-click editing with collaborators.

1. Go to sites.google.com and start a blank site named "AQuA Research Lab".
2. Create pages: Home, Research (with one subpage per project or poster), Approach, Work with us.
3. Copy the text from each HTML page in order, section by section. Use Google Sites headings (Title, Heading, Subheading) to match the H1/H2/H3 structure.
4. Recreate status labels as bold text at the top of each project section, for example **Status: Proposed research**, so status stays visible.
5. Upload the SVG illustrations (or PNG exports of them) with alt text; keep the "Conceptual illustration" captions.
6. Use buttons only for links that work. Do not add the contact button until the address is approved.
7. Share edit access with named lab members only, then click **Publish**. Check the published site on a phone.

## 7. Before every significant change

- Is every new factual statement true and approved for public use?
- Does each project still show its correct status?
- Do the new links work on a phone, using only the keyboard (Tab and Enter)?
- Does new text avoid promising results, funding, positions or outcomes the lab does not have?
