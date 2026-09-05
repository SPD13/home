# spd13.us

Static homepage for [spd13.us](https://spd13.us), published with GitHub Pages from this repo.

## Structure

- `index.html` – the page (header + project tiles)
- `assets/style.css` – styles
- `assets/screenshots/` – project screenshots (see the README inside)
- `CNAME` – custom domain for GitHub Pages
- `.nojekyll` – serve files as-is, no Jekyll processing

## Adding a project

Copy the `<article class="tile">` block in `index.html`, update the title, description,
links and screenshot file name, then add the screenshot to `assets/screenshots/`.

## Publishing

In the repo settings on GitHub: **Pages → Source: Deploy from a branch → `main` / `/ (root)`**.
The `CNAME` file sets the custom domain to `spd13.us`.
