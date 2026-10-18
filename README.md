# Citadel CX website

Astro static site, deployed on Cloudflare Pages, with Sveltia CMS for non-technical editing.

## Run it locally

```powershell
npm install
npm run dev
```

Opens on http://localhost:4321.

```powershell
npm run build
```

Writes static files to `dist/`.

## Where the content lives

Page text is in `src/content/pages/*.json`. The page structure is in `src/pages/*.astro`.
Changing wording means editing the JSON; changing layout means editing the Astro file.

Job openings are markdown files in `src/content/jobs/`. The careers page shows
"not hiring yet" when that folder is empty, and a list of roles when it is not.

Nav and footer live in `src/layouts/Base.astro` only. Adding a page means:

1. a new file in `src/pages/`
2. a new JSON file in `src/content/pages/` for its wording
3. one line in the `ways` array in `Base.astro`
4. a new `files:` entry in `public/admin/config.yml` so the CMS can edit it

## Deploying to Cloudflare Pages

1. Push this repo to GitHub.
2. Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git.
3. Build command: `npm run build`. Output directory: `dist`.
4. Custom domains → add `www.citadelcx.com`, then `citadelcx.com`.

Cloudflare rebuilds and redeploys on every push to `main`.

## Turning on the CMS

The editor lives at `/admin`. Two things to do before it works:

**1. Point it at your repo.** In `public/admin/config.yml`, change the `repo:` line
from `YOUR-GITHUB-USERNAME/citadelcx` to the real path.

**2. Set up GitHub OAuth**, so editors can sign in:

- GitHub → Settings → Developer settings → OAuth Apps → New OAuth App
- Homepage URL: `https://www.citadelcx.com`
- Authorization callback URL: the URL of your auth worker (below)
- Note the Client ID and Client Secret

Then deploy a small Cloudflare Worker to handle the token exchange. Sveltia
documents this; `sveltia-cms-auth` is a ready-made Worker that does it. Set the
Worker's `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET` as secrets, and put the
Worker's URL in `config.yml` as `base_url`.

Until that's done, the site builds and deploys fine — only `/admin` is inactive.

Each editor needs write access to the GitHub repo. If you'd rather they didn't,
put Cloudflare Access in front of `/admin` and use a single shared GitHub
machine account.

## Design notes

- One typeface, Archivo variable, using the width axis for headline character
- Petrol ground, brass accent, verdigris used sparingly; brass appears as rules,
  marks and small fills
- No page frame and no rules between sections — sections separate by alternating
  background (`.bay` / `.bay-sunk`)
- Cards (`.cell`) are spaced panels with a brass top edge
- Numbered markers only where content is a genuine sequence (how-it-works steps)
- Light and dark both supported; every colour is a CSS custom property with a
  `[data-theme="light"]` override; brass fills swap to ink on pale backgrounds
- Motion is one page-load sequence in the hero, disabled under `prefers-reduced-motion`
