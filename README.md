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

**Not implemented yet:** server-side recurring sync, comprehensive analytics dashboards, restored playlist versions, discography completion, advanced merges/splits, multi-device accounts, and public-ready analytics compliance review. The site does **not** silently track plays in the background while closed. Spotify API listening analytics are deliberately not implemented; currently statistics come from the user's own downloaded archive only.

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
- **Exact duplicates:** recordings from different releases may have different URIs. The in-place removal tool retains the first occurrence of each URI and only removes later repeats. It requests position-specific removal and saves a browser-local backup before making changes. Review the confirmation carefully, since it changes your Spotify playlist directly.
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
