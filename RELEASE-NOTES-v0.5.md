# SoundLab v0.5 — Visual Analytics & Duplicate Scan Progress

## Added
- Playlist Duplicate Detective now shows page-by-page progress, counts and percentage, and only shows scan findings after the scan finishes.
- Listening Analytics with All time / Day / Week / Month / Year / Custom UTC date filters.
- Ranked artist, track and album sections with on-demand Spotify artwork loading; artist images and album artwork link to Spotify.
- Listening activity charts and 24-hour heatmap.
- Downloadable 1080 × 1350 PNG recap, generated locally using imported records (text-based; Spotify artwork is not embedded in the recap).
- Preserves history importer, playlist tools, album journal and duplicate fix.

## Not yet included
- Background listening capture is NOT implemented. GitHub Pages alone cannot run background tasks; an approved, secure backend is needed.
- Online storage, cross-device sync, and always-live analytics are NOT present.
- Artwork lookup is manual and requires connected Spotify, and can fail due to Spotify limits/availability.

## Testing
1. Push the whole project to GitHub Desktop, commit, push.
2. Wait for GitHub Pages Actions to finish. Hard refresh the site.
3. Select a long playlist and watch the progress bar update in steps of up to 50 items. Do NOT test edits on irreplaceable playlists.
4. In Listening Analytics select periods and try on-demand artwork and Export recap PNG.

Data stays in the same browser's IndexedDB/localStorage. Do not clear site data.
