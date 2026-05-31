# Certifyd Preview Deployment (GitHub Pages + Cloudflare)

This runbook sets up a separate preview deployment for marketing edits while keeping production (`certifyd.me`) untouched.

## 1. Repository split (required)

1. Create a **new repo** for preview, for example: `certifyd-me-site-preview`.
2. Push this site code to that preview repo.
3. Keep production DNS and production Pages repo unchanged.

## 2. Files required in preview repo

These are now present in this workspace and should exist in the preview repo root:

- `robots.txt`

```txt
User-agent: *
Disallow: /
```

- `CNAME`

```txt
preview.certifyd.me
```

- `<meta name="robots" content="noindex,nofollow">` in each page head/layout (`index.html`, `network.html`).

## 3. GitHub Pages settings (preview repo)

In the **preview repo** on GitHub:

1. Go to `Settings` -> `Pages`.
2. `Source`: Deploy from branch.
3. Branch: `main` (or preview branch), folder `/ (root)`.
4. Save.
5. In the same Pages section, set custom domain to `preview.certifyd.me`.
6. Keep `Enforce HTTPS` on after DNS resolves.

## 4. Cloudflare DNS (certifyd.me zone)

Create DNS record:

- Type: `CNAME`
- Name: `preview`
- Target: `<your-github-username>.github.io`
- Proxy status: DNS only (gray cloud) recommended for fastest Pages validation

If you must proxy through Cloudflare later, switch after SSL/Pages is fully healthy.

## 5. Security/content hygiene

Preview is public-by-link only, not private.

Before publish, verify preview does **not** include:

- secrets
- API keys
- investor-only terms
- private roadmap notes
- private partner claims

## 6. Production safety

Production remains unchanged when all three are true:

1. Production domain (`certifyd.me`) keeps existing DNS + existing Pages repo.
2. Preview uses separate repo + `preview.certifyd.me` CNAME.
3. Only preview repo receives staging edits.
