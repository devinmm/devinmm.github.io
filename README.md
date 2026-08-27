# mulrooney.me

Personal site and blog. Hugo + PaperMod, deployed to GitHub Pages via Actions.

## Local development

```bash
hugo server -D          # includes drafts, live reload at localhost:1313
hugo --gc --minify      # production build into ./public
```

## Writing a post

```bash
hugo new content posts/my-post.md
```

Posts start with `draft: true`. Flip it to `false` (or delete the line) to publish.

## First-time setup

1. Create a **public** GitHub repo and push this directory to `main`.
2. Repo → **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. Push. The workflow in `.github/workflows/hugo.yml` builds and deploys.

### Custom domain (mulrooney.me)

1. Add a `static/CNAME` file containing `mulrooney.me` (already present).
2. At your DNS provider, point the domain at GitHub Pages:
   - `A` records for the apex → `185.199.108.153`, `185.199.109.153`,
     `185.199.110.153`, `185.199.111.153`
   - or a `CNAME` for `www` → `devinmm.github.io`
3. Repo → Settings → Pages → Custom domain → enter `mulrooney.me`, enable
   **Enforce HTTPS** once the cert provisions.
4. `baseURL` in `hugo.yaml` is already set to `https://mulrooney.me/`.

## Theme

PaperMod is a git submodule. Clone with:

```bash
git clone --recurse-submodules <repo-url>
```

Update it with:

```bash
git submodule update --remote --merge themes/PaperMod
```

## TODO

- [ ] Add favicon files to `static/`
- [ ] Leave `static/CNAME` in place — GitHub needs it on every deploy
- [ ] Finish and publish the PatchPilot post
