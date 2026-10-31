# SoundLab 🎧

A **standalone** Spotify playlist management website, separate from Playlist Paster. Built with plain JavaScript, Vite and JSZip, with a green/black responsive UI.

## What's working in this first release

- Spotify account connection using **OAuth 2.0 Authorization Code + PKCE** (no client secret).
- Browse owned and followed playlists; inspect track lists.
- Detect exact duplicates by Spotify URI; **remove duplicate occurrences from the original owned playlist**, with confirmation and a local backup before editing.
- Save local playlist track-list backups and download them as JSON.
- Built-in Playlist Paster: paste `Song - Artist` lines, review Spotify matches, create a playlist.
- Optional Spotify **Extended Streaming History** ZIP/JSON import, stored locally using IndexedDB.
- Re-import historical archives with idempotent record fingerprints, view listening hours and top artists/tracks, and export/delete your imported records.
- Local album journal with notes, status and ratings.
- Start-now vs. import-later onboarding selection.
- Roadmap panels for more advanced features.

**Not implemented yet:** comprehensive analytics dashboards, restored playlist versions, discography completion, advanced merges/splits, multi-device accounts, and public-ready analytics compliance review. The site does **not** track plays in the background. Spotify API-derived listening analytics are deliberately excluded because Spotify Developer Policy restricts creating listening metrics or user profiles from Spotify Platform data; currently statistics come from the user's own downloaded archive only.

## Quick GitHub Pages deployment

1. Unzip the SoundLab folder and add **all its contents** to a new GitHub repository named `SoundLab` (keep the `.github/workflows/deploy.yml` folders).
2. Ensure the repository's default branch is **main**.
3. GitHub → repository **Settings → Pages → Build and deployment → Source: GitHub Actions**.
4. The included GitHub Actions workflow installs dependencies, builds the site and publishes it. After it finishes, the usual URL is `https://YOUR_USERNAME.github.io/SoundLab/`.
5. Go to [Spotify Developer Dashboard](https://developer.spotify.com/dashboard), create a Spotify app, and add your deployed **exact URL** as a Redirect URI. For example `https://YOUR_USERNAME.github.io/SoundLab/` (including trailing slash).
6. Open the deployed website → **Settings & Sync** → paste the Spotify **Client ID only** → Save → Connect Spotify. **Never paste/share your Client Secret.**
7. With Spotify development-mode restrictions, add the Spotify account you intend to use to your developer app's allowlist. Spotify may require the app owner to have Premium and limits development-mode users.

### Run locally

Install [Node.js](https://nodejs.org/) v20.19+ or v22+, then run:

```bash
npm install
npm run dev
```

Open the address Vite prints (typically `http://127.0.0.1:5173/`). Register **that exact URL** as an additional Spotify Redirect URI. Spotify does not permit `http://localhost:5173/` as a redirect URI.

### GitHub web upload caveat

GitHub's web uploader may hide dot-folders or make them difficult to select. Ensure `.github/workflows/deploy.yml` and `.gitignore` are included. You can also upload the contents through GitHub Desktop.

## Data handling and warnings

- Listening history is processed in-browser; streaming records are kept in **IndexedDB** on the current browser/device, not on a server. Importing the same export again replaces identical record fingerprints instead of duplicating them. Spotify data export formats can vary; if you see an unrecognised format, adapt `normalise()` in `src/main.js`.
- Records are filtered to actual music-track records. Their IP addresses and device identifiers are **not** imported into the local database.
- Imported data are not synced between devices. Clearing browser site data can remove both history and journal. Download exports for backups.
- Spotify login tokens live in session storage and are scoped to the current browser tab session. This is a client-side prototype, not an audited production multi-user service.
- Playlist backup export contains metadata/URIs only, never audio content.
- **Exact duplicates:** recordings from different releases may have different URIs. The in-place removal tool retains the first occurrence of each URI and only removes later repeats. Spotify no longer reliably supports position-specific URI deletion. The tool deletes all appearances of a repeated URI and immediately inserts one at its original adjusted position, verifying every stage. This may reset the retained entry’s Spotify Date Added. A browser-local backup is saved before making changes. If Spotify rejects a request midway, the cleanup stops and the user should inspect the playlist and backup before retrying. Review the confirmation carefully, since it changes your Spotify playlist directly.
- The search-based Paster selects the first search result; always review the matches, which can be imperfect.
- Spotify API permissions and endpoint availability can change and may differ by development-mode app.
- Spotify platform rules restrict some derived listening metrics. Before public release, get a compliance review and confirm that the intended use of user-provided exports is permitted by relevant terms.

## Architecture

- `index.html` – application entrypoint
- `src/main.js` – UI, Spotify API client, playlist operations, local IndexedDB import and journal
- `src/style.css` – responsive dark styling
- `vite.config.js` – relative build paths for GitHub Pages
- `.github/workflows/deploy.yml` – automatic GitHub Pages deployment

## References

- [Spotify PKCE](https://developer.spotify.com/documentation/web-api/tutorials/code-pkce-flow)
- [Spotify redirect URI requirements](https://developer.spotify.com/documentation/web-api/concepts/redirect_uri)
- [Spotify February 2026 Web API migration](https://developer.spotify.com/documentation/web-api/tutorials/february-2026-migration-guide)
- [Spotify understanding your data](https://support.spotify.com/au/article/understanding-your-data/)

Independent project. Not affiliated with, endorsed by or sponsored by Spotify.


### Album Journal lookup (v0.4)
Open **Album Journal**, enter an album or artist in **Search Spotify albums**, select a result, then add your review and save. This uses the Spotify `/search?type=album` API and requires an authenticated Spotify connection. The album title, artist, release date, Spotify album URL, cover, and track count are stored in your browser. Existing journal entries are preserved. Manual entry works without Spotify.

## v0.5 upgrade: Visual Analytics and scan progress

- Duplicate Detective shows an updating **page-by-page progress bar** while loading a playlist. Spotify returns up to 50 items per page; progress is displayed from the API's reported total and can briefly stay near 99% until the last page is complete. The scan does **not** remove songs; duplicate removal still requires confirmation.
- Listening Analytics includes day/week/month/year/custom UTC date filters, top artists/tracks/albums with optional on-demand Spotify image lookup, an hourly chart and a text-based shareable PNG recap. The PNG is rendered in the browser from the user's imported data, without downloading or modifying Spotify cover art.
- These features work with **imported** listening history only. There is no automatic tracking of newly played songs when the site is closed.
- If importing historical data on a different device, export it from the old browser and re-import it in the new browser.
- To update an existing installation, copy the contents of the extracted `SoundLab` folder into your local GitHub clone, preserving `.github/workflows/deploy.yml`, then Commit and Push in GitHub Desktop.



## v0.7 — Imported-history-only analytics
- Removed the Recent Activity page, its Web API fetch, and the `user-read-recently-played` OAuth permission.
- Removed automated listening capture from the roadmap and clarified in the app that Spotify's developer policy restricts generating new listening metrics/profiles from Spotify Platform data. See https://developer.spotify.com/policy .
- **Listening Analytics:** generated solely from explicitly uploaded Extended Streaming History ZIP/JSON exports. No background stream tracking, recent-play polling, or promises of real-time statistics.
- **Playlist management:** Spotify authentication, playlist refresh, Duplicate Detective, album search, and other existing permitted tools remain available. Refreshing playlists does not track listening activity.
- Import newer history files whenever available. This is a local-first prototype; before launching a public service, independently review the terms applicable to user-exported data as well.
- Existing imported records, saved journal and settings remain in browser storage under their previous keys.


## v0.8 — automatic images and local timezone
Listening Analytics automatically requests Spotify artwork where available. Choose your IANA display timezone on Analytics or in Settings; default Australia/Brisbane. Imported UTC timestamps are preserved. This does not enable Spotify background listening tracking. See RELEASE-NOTES-v0.8.md.
