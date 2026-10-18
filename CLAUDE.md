# Citadel CX — website

Marketing site for Citadel CX, a Canadian company delivering customer support
through a team in Lagos working on managed virtual desktops hosted in Canada.

Five pages: home, how it works, about, careers, contact.

## Stack

- **Astro** (static output) — no framework runtime ships to the browser
- **Cloudflare Pages** — free hosting; currently direct upload of `dist/`
- **Sveltia CMS** — built at `/admin`, not yet active (needs repo path + OAuth)
- Domains and DNS at Cloudflare; custom domain not attached yet

```powershell
npm run dev     # localhost:4321, live reload
npm run build   # writes dist/
```

Node 22.22.0 or higher. The user is on Windows — give commands in **PowerShell**,
never bash.

## Where things live

| What | Where |
|---|---|
| Page copy | `src/content/pages/*.json` |
| Page structure | `src/pages/*.astro` |
| Nav, footer, theme script | `src/layouts/Base.astro` (one place only) |
| Styles | `src/styles/global.css` |
| Logo, hero art | `src/components/` |
| Job openings | `src/content/jobs/*.md` |

Changing wording means editing JSON. Changing layout means editing the Astro file.
The careers page shows "not hiring yet" when `src/content/jobs/` is empty and a
list of roles when it is not — that switch is automatic, don't hard-code it.

Adding a page means: new file in `src/pages/`, new JSON in `src/content/pages/`,
one line in the `ways` array in `Base.astro`, and a new `files:` entry in
`public/admin/config.yml`.

## Copy rules — these matter

**Never claim client data does not leave Canada.** It is not true: an agent in
Lagos reading a customer record is a transfer for processing regardless of where
the bytes rest, and PIPEDA has no data-localisation requirement anyway. Accurate
framing is "hosted in Canada" and "does not persist on agent devices". A client's
privacy counsel will take apart anything stronger.

**Lagos is disclosed openly** on the about and careers pages. That is deliberate —
concealment discovered later would be worse. Do not quietly remove it.

**No pricing, no seat counts, no named verticals.** The company's positioning is
still unresolved between serving small businesses directly and being the human
escalation layer for AI receptionist platforms. Copy should stay true under
either. Avoid committing the site to one.

Tone: plain, specific, no marketing inflation. Sentence case. Short sentences.

## Design system

- **One typeface**: Archivo variable, using the width axis for headline character
- **Petrol ground, brass accent** (`--brass`), verdigris used sparingly
- Brass appears as **rules, marks and small fills** — on light backgrounds the
  primary button switches to ink because brass fill goes muddy
- **No page frame, no gridlines.** Sections separate by alternating background
  (`.bay` / `.bay-sunk`), not borders. This was a deliberate removal — don't
  reintroduce rules between sections.
- Cards (`.cell`) are spaced panels with a brass top edge, not a bordered grid
- Numbered markers only where content is genuinely a sequence (the how-it-works
  steps). Not on card grids.
- Motion: one page-load sequence in the hero, nothing else. All of it wrapped in
  `prefers-reduced-motion`.

## Light and dark

Both are supported and must both work. Theme is set by an inline script in
`Base.astro` before paint, follows the system setting until the user clicks the
toggle, then their choice persists in `localStorage`.

Any new colour goes in as a CSS custom property with a `[data-theme="light"]`
override. Never hard-code a hex in a component.

## Accessibility floor

- Touch targets 44px minimum under `@media (pointer: coarse)` — inline links
  inside sentences are exempt and should stay exempt
- The mobile menu uses `aria-expanded`, `aria-controls`, Escape to close
- Visible focus rings (`:focus-visible`) — don't remove them
- Verified no horizontal overflow at 320, 375, 393, 768, 1280

Re-check overflow and tap targets after any layout change.

## Not done yet

- Custom domain not attached (deploying to `*.pages.dev` for partner review)
- `/admin` inert until `public/admin/config.yml` gets the real GitHub repo path
  and an OAuth worker is deployed
- Not in git yet; moving to GitHub + Cloudflare Git integration is the next step
- `info@` and `careers@` addresses do not receive mail yet (M365 licence issue)
