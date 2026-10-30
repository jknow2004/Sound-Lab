# SoundLab v0.3 — GitHub Pages testing release

This is a standalone project, independent of Playlist Paster.

## Newly implemented in this release

- Playlist DNA: compare two playlists and view shared and exclusive track counts.
- Playlist Mixer: preview and create a new playlist from two playlists, with optional exact-URI deduplication.
- Playlist Sorter: preview and reorder an owned playlist by title, artist, album or reverse sequence, with an automatic local backup.
- Global Search: find tracks/artists across accessible playlist contents, on demand.
- Music Vault: restore local playlist backups to the original playlist, with a confirmation and an automatic pre-restoration backup; import/export backup files.
- Playlist Architect: save and export original story/chapter plans locally.
- Listening history: additional yearly/hourly insights and forgotten-favourites discovery from user-imported plays.
- Album journal: export entries as JSON.
- Earlier duplicate-removal fix is included: this edits the existing playlist, preserves a single URI occurrence and verifies the result.

## Important limitations

- Automatic background Spotify listening history tracking is **not running**. GitHub Pages serves static files only. There is no scheduled backend, database or continuous capture while the website is closed.
- The history import feature reads user-provided JSON or ZIP files in this browser. Data is stored in IndexedDB on this device only; refresh/import again on another device.
- Spotify restricts some uses of Spotify Platform data for derived user analytics. Production deployment of any automatically collected play-based analytics requires separate policy review.
- Playlist read/write functions rely on the Spotify Web API and require applicable permissions, ownership and Spotify app access. Spotify may change API behaviour or rate-limit requests.
- Sorting and restoring may change `Date Added` values in Spotify. Playlist changes are not atomic; if a request fails midway, restore from the original backup.
- Spotify local/unavailable items may prevent some editing operations to avoid destructive outcomes.
- No claims of an automatic sync, public multi-user support, comprehensive discography lookup, complete AI features, or verified live Spotify end-to-end testing are made.

## Install / update from ZIP

1. Extract the ZIP.
2. Open GitHub Desktop -> Repository -> Show in Finder for your existing `Sound-Lab` repo.
3. Copy the **contents of `SoundLab`**, including `.github` and `src`, into the repository; replace matching files.
4. In GitHub Desktop, enter a summary such as `SoundLab v0.3 power tools`, then **Commit to main** and **Push origin**.
5. In GitHub -> Actions, wait for a green deployment check, then reload the site.
6. Test destructive operations on disposable playlists first. Download a Vault backup before editing important playlists.

## Development

Requires Node.js 22. `npm install` then `npm run dev`; `npm run build` builds production files.

The Spotify Client ID already saved in your browser's local storage for the same GitHub Pages URL should remain there after redeployment. Never put a Spotify Client Secret in source code.


## v0.4 — Album Journal Spotify Search
- Search Spotify's album catalogue directly inside Album Journal (requires Spotify sign-in).
- Select a result to auto-fill title, artist, release date, track count, Spotify link, and cover image.
- Add your own rating, listening status, and review; save metadata in the existing local album journal.
- Previously saved journal entries are preserved and remain viewable.
- Manual album entry continues to work without Spotify.
- Search is limited to 10 results per request to match development-mode restrictions.
- Album artwork links to Spotify; Spotify-sourced metadata requires attribution.
