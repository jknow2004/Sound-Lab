# SoundLab v0.8 — Automatic artwork and local timezone

- Artwork for the top eight artists, albums and songs is now requested automatically when Listening Analytics opens and Spotify is connected. No manual artwork button is needed.
- Matching artwork is cached locally to reduce API calls. If Spotify is disconnected or blocks an image lookup, the dashboard keeps a placeholder instead. Artwork is separate from listening-statistics computation.
- Added an IANA timezone selector in both Listening Analytics and Settings & Data; default is Australia/Brisbane.
- Day/week/month/year/custom filtering, yearly and hourly aggregations, history coverage, and recap date labels now follow the selected timezone. Original imported timestamps remain UTC, and historical data is not rewritten.
- Timezone selection stores a timezone ID, not a precise address.
- Historical imports and playlist management are retained. Background listening tracking is still not available.

## Updating
Copy the contents of this folder into your existing Sound-Lab GitHub repository, replacing matching files, then commit and push. Clear browser cache / hard refresh if changes do not appear.

## Caveats
Automatic images require an active authenticated Spotify connection and the Spotify Search API. API availability and rate limits may affect some images. Historical uploads and existing ratings are stored in the browser and are not packaged with the downloaded files. Test the date boundaries using a stream near midnight UTC.
