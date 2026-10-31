# SoundLab v0.6 — Recent Activity & History Coverage

## Added
- Dedicated **Recent Activity** page showing the most recent Spotify playback entries, artwork, artist, album, and playback time.
- **Refresh recent activity** button that makes a live, on-demand Web API request.
- Historical import coverage date and a visible warning when the imported archive is older than today.
- OAuth permission `user-read-recently-played` added. **Existing users must disconnect and reconnect** before recent plays can be accessed.
- Graceful 401/403 errors if the endpoint isn't authorised or allowed in the developer app's access mode.

## Not implemented (and why)
- **No background streaming collection**, automated polling, persistent recent-play records, or statistics derived from Spotify API playback data. The developer policy (https://developer.spotify.com/policy) prohibits deriving listening metrics from Spotify Platform data.
- **GitHub Pages cannot run a scheduled background process**. A server would be required even if Spotify explicitly approved the desired use.
- Real analytics continue to use user-uploaded streaming history only. Spotify export archives can be re-imported and deduplicated.

## Upgrade from v0.5
Copy the contents of this SoundLab folder into your existing GitHub Desktop repository, replacing matching files. Commit and push. No new Spotify Developer application or redirect URI needed. Reconnect inside SoundLab so Spotify can grant the new scope.

## Security
Read-only recent-play data stays in JS memory until page refresh and is not written to IndexedDB or localStorage.
