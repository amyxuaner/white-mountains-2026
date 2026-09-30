# White Mountains Fall Trip Handbook: GitHub → Cloudflare Workers

The site is live: [open the White Mountains Fall Trip Handbook](https://white-mountains-2026.amyxuaner.workers.dev/). The first Cloudflare build succeeded, the production URL was opened in a browser, and requests without login credentials return HTTP 200; the live content matches the delivered HTML (trailing newline only).

The public GitHub repository [amyxuaner/white-mountains-2026](https://github.com/amyxuaner/white-mountains-2026)'s `main` branch is connected to the Cloudflare Worker `white-mountains-2026`. From now on, editing `public/index.html` and committing or pushing to `main` triggers a release to the same URL. Each release can be checked against its commit in Cloudflare Builds.

This package contains no original booking confirmations, screenshots, or private credentials. The source repository is public, the travel site is public, and companions need no login and no app install.

## What you need to do now

One-time setup — repository creation, page upload, GitHub connection, and the first deployment — is already done. **There are no remaining one-time deployment steps, and nothing that must be done on a computer.**

- Optional: bookmark the URL above on your phone and forward it directly to your travel companions.
- Optional: save the delivered single HTML file, or use the page's "save offline copy" button; before departure, open the file with the network off to confirm it works.
- Later: when todos are done or the itinerary changes, update the page and commit; no need to recreate the repository or reconnect Cloudflare.

## Files and usage

```text
white-mountains-2026/
  public/
    index.html       ← the only page file: CSS, JavaScript, SVG all inlined
  wrangler.jsonc     ← Cloudflare Workers static-site config
  .gitignore
  部署说明.md         ← this guide (Chinese)
  DEPLOYMENT.md      ← this guide (English)
```

Open `public/index.html` directly in a browser to preview. About 85 KB; no app install needed, no external fonts, scripts, or images requested. When the browser runs JavaScript, the countdown updates every second; file previewers without JavaScript still show the static itinerary.

"Save offline copy" downloads the single file. If an app's built-in browser doesn't support downloads, save the delivered HTML directly, or open the public URL in Safari / Chrome to download it. Google Maps navigation, food ordering, and official links need network access; download the region in Google Maps separately. Having visited the online URL once does not guarantee it will reload offline.

## Rebuild reference: current deployment settings and steps

For future recovery or rebuilds only — the site is already live, no need to repeat these steps.

1. **Stay logged in to GitHub and Cloudflare.** The GitHub repository already exists; the Cloudflare Free plan is enough. Passwords, verification codes, and account terms are yours to handle — don't paste them into a chat.
   - GitHub repo: [amyxuaner/white-mountains-2026](https://github.com/amyxuaner/white-mountains-2026)
   - Cloudflare: [dash.cloudflare.com](https://dash.cloudflare.com/)
2. **Verify the repository contents.** Open the existing repository, switch to `main`; the committed core files are `public/index.html`, the root-level `wrangler.jsonc`, and `.gitignore`. To restore from this package later, upload the matching files via GitHub's Upload files page and commit to `main`. Don't nest an extra `white-mountains-2026` folder. Don't upload the ZIP itself, and never upload original bookings, screenshots, QR codes, email exports, or `work` folders.
3. **Connect Cloudflare to the repository.** Open **Workers & Pages → Create application → Import a repository / Get started**, choose GitHub. Authorize **Cloudflare Workers and Pages** with the repository scope limited to **amyxuaner/white-mountains-2026**.
4. **Fill in the deployment settings:**
   - Worker name: `white-mountains-2026`, matching `name` in `wrangler.jsonc`.
   - Production branch: `main`.
   - Root directory: `/` (repository root).
   - Build command: **leave empty**.
   - Deploy command: **`npx wrangler deploy`**.
   - No environment variables, databases, API keys, build output directory, or paid plan needed.
5. **Deploy.** On rebuild, click Save and Deploy and wait for success. Already deployed successfully; the actual production URL is [white-mountains-2026.amyxuaner.workers.dev](https://white-mountains-2026.amyxuaner.workers.dev/). Keeping the existing account and Worker name keeps this URL.
6. **Public access.** The production URL is enabled with Cloudflare Access off. After a rebuild, confirm there's no login page using a phone or incognito window, then test the day-route expanders, Google navigation, the per-second countdown, and the offline download.
7. **Automatic releases afterwards.** Just edit `public/index.html`, commit or push to `main`, and Cloudflare Workers Builds will trigger a deployment. Confirm the commit shows as successful in the Worker's Builds list, then refresh the public URL.

### Which steps need a computer?

- Nothing remaining strictly requires a computer. For a future rebuild, account signup, authorization, and console deployment have no platform-mandated "must use a computer" step — a mobile browser can theoretically do it.
- **Steps 2–5 are strongly recommended on a computer**, especially unzipping and uploading the complete `public` folder; mobile file pickers easily break the directory structure.
- If you choose the command-line Git option below, **the local terminal steps must be done on a computer**.
- After one deployment, companions only need a mobile browser on the public link — no account or app.

## If you know Git: optional terminal workflow

The repository already exists — don't re-initialize or overwrite it. After logging in to GitHub the normal way, clone the existing repository on your computer:

```sh
git clone https://github.com/amyxuaner/white-mountains-2026.git
cd white-mountains-2026
```

The repository already has the page and deployment config, and Cloudflare is already connected. Edit the cloned `public/index.html`, or replace it with an updated single file, then commit the update:

```sh
git add public/index.html
git commit -m "Update travel itinerary"
git push origin main
```

Editing `public/index.html` directly in the GitHub web UI and committing works the same — either triggers a deployment.

## Content maintenance

- The packing list is an interactive checklist: checking items updates the progress counter, and the state persists per device via `localStorage`. (The Chinese guide still describes the old plain list.)
- All timestamps carry `-04:00` and display as America/New_York / EDT. Don't rewrite times as timezone-less strings.
- Automatic day/night theming follows the viewer's device hour: 07:00–18:59 day, otherwise night; it can also be chosen manually, with the preference stored in the current browser only.
- Google Maps mobile web handles at most 3 waypoints. Day 2 keeps the full navigation link, plus morning/afternoon segment links as fallback.
- Public content carries itinerary essentials only — no confirmation codes, QR codes, door locks, Wi-Fi passwords, room numbers, or personal contact details.
- Restaurants remain recommendations; nothing is reserved or ordered. Loon 10:00 AM is a planned time; verify Cathedral Ledge road status and orchard picking conditions before departure.
- The orchard stop is Applecrest Farm Orchards, planned 1:00–3:00 PM on Oct 12; their site confirms a holiday harvest festival that day. The drive home can shift later with traffic.

## Free plan and official docs

This project hosts static HTML only — no Worker dynamic compute, paid databases, or external service calls. Per Cloudflare's current terms, static asset requests are free and unlimited; Workers Builds Free includes 3,000 build minutes per month. Keep the static configuration and don't enable extra paid products.

- [Workers static assets config](https://developers.cloudflare.com/workers/static-assets/binding/)
- [GitHub connection & automatic releases](https://developers.cloudflare.com/workers/ci-cd/builds/)
- [Build configuration](https://developers.cloudflare.com/workers/ci-cd/builds/configuration/)
- [Static assets billing & limits](https://developers.cloudflare.com/workers/static-assets/billing-and-limitations/)
- [Build free quota](https://developers.cloudflare.com/workers/ci-cd/builds/limits-and-pricing/)
- [workers.dev public access](https://developers.cloudflare.com/workers/configuration/routing/workers-dev/)

If a build ever complains about a Worker name mismatch, keep both names identical; if it can't find `public`, check whether the repo has a nested folder. Day-to-day updates never need deployment config changes.
