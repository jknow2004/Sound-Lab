# SoundLab v0.7 — Import-only Listening Analytics

## Changes
- Removed Recent Activity page and recently-played API calls.
- Removed `user-read-recently-played` OAuth scope; there is no background listening synchronisation or Spotify API-derived stream counting.
- Settings explain the Spotify Developer Policy limitation and link to the policy.
- Listening statistics update **only when the user imports their own history export**.
- Playlist refresh and playlist management still function independently of listening history.
- Existing browser-local data and settings are preserved.

## Update via GitHub Desktop
Copy **the contents of `SoundLab/`**, including the hidden `.github` folder, into your existing `Sound-Lab` clone. Replace matching files. Commit and push. You do not need to create a new Spotify developer app.

## Policy caveat
Current Spotify Developer Policy restricts deriving new listening metrics or profiles from Spotify Platform data. The app does not use the Spotify API for analytics. Imported exports are treated separately, but public/commercial release still requires reviewing applicable terms and privacy obligations.
