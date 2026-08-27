# Setup

The PaperMod theme is vendored in `themes/PaperMod`, so this runs as-is.
The site is already configured for **https://mulrooney.me**.

## Try it locally

```bash
cd blog
hugo server -D          # drafts included, live reload at localhost:1313
```

Requires Hugo **extended** 0.146.0 or newer. On Ubuntu, grab the `_extended_`
.deb from the Hugo releases page — the version in apt is usually too old.
The GitHub Actions workflow is pinned to 0.151.0.

## Before you push

- [ ] Finish `content/posts/patchpilot.md` and set `draft: false`

## Publish to GitHub Pages

```bash
cd blog
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/devinmm/devinmm.github.io.git
git push -u origin main
```

Then: repo → **Settings → Pages → Build and deployment → Source: GitHub Actions**.

The repo must be **public** for Pages on a free account.

**Repo name:** naming it `devinmm.github.io` makes this your GitHub *user site*,
which is the cleaner fit for a personal domain. Any other name works too — it just
serves from a subpath until the custom domain is attached.

## Custom domain: mulrooney.me

`static/CNAME` already contains `mulrooney.me`. Do not delete it — without it,
GitHub resets the custom domain on every deploy.

### DNS records

At your registrar, for the apex (host `@`), add four A records:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Optionally add AAAA records for IPv6 (also host `@`):

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

For `www`, add one CNAME:

```
Host: www    →    devinmm.github.io
```

**Leave your MX records alone.** mulrooney.me handles email; A records for the
apex do not affect mail delivery.

### GitHub settings

Repo → **Settings → Pages** → Custom domain → `mulrooney.me` → Save.
Wait for the DNS check to pass, then enable **Enforce HTTPS**. The Let's Encrypt
cert is provisioned automatically; the checkbox stays greyed out until GitHub
verifies the domain, which is normal.

### Verify

```bash
dig +short mulrooney.me          # should return the four GitHub IPs
dig +short www.mulrooney.me      # should return devinmm.github.io
```

DNS propagation is usually minutes, occasionally up to 24 hours.

## Writing

```bash
hugo new content posts/my-post.md
```

New posts start as `draft: true`. Flip to `false` to publish.
