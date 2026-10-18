<div align="center">

<img width="384" height="384" alt="icon" src="https://github.com/user-attachments/assets/2ebbe454-d828-440d-948b-35ff610aa338" />

# Openify Music

**A free music player in a single HTML file.**

Stream anything on YouTube Music, browse today's Spotify Global Top 200 and sing along with synced lyrics. There's no account, no ads and no install.

<br>

![Single file](https://img.shields.io/badge/single-file-1eb1d6?style=for-the-badge)
![No account](https://img.shields.io/badge/account-not%20needed-21a1c9?style=for-the-badge)
![Chart](https://img.shields.io/badge/chart-updated%20daily-2a8fb8?style=for-the-badge)
![License](https://img.shields.io/badge/license-GPL--3.0-3b7ca6?style=for-the-badge)

<br>

**[Open Openify Music](https://ltorl.github.io/openify-music/)**

</div>

<br>

## Features

<table>
<tr>
<td width="50%" valign="top">

### Global Top 200, every day
- The home page is the **Spotify global daily chart**, fetched fresh from [kworb.net](https://kworb.net/spotify/country/global_daily.html) every time the page loads.
- Every song shows its **rank**, **movement** since yesterday and **streams today**, and the number one gets a big hero card.
- Press play on any song and the chart keeps going **in chart order**.
- Each chart song is matched to its YouTube Music track once and then remembered, so only songs new to the chart are searched.

### Search & play
- Search songs and artists across **YouTube Music**.
- A **now-playing bar** at the bottom has play / pause, skip, a seek bar and volume.
- Open the full **player** for big artwork and controls.

</td>
<td width="50%" valign="top">

### Synced lyrics
- Lyrics scroll along with the song, line by line.
- Toggle **romanization** and **translation** for songs in other languages.
- Download the lyrics as **LRC**, **TTML** or plain text.

### Your library
- Tap the heart to save a song to **Your Library**.
- Make **playlists** and add songs with the **+** button, right next to the heart.
- Everything stays in your browser. Nothing gets uploaded and you don't need an account.

### Anywhere
- The whole app is **one `index.html`**. Open it from GitHub Pages, any static host, or straight from your disk.
- It has a **mobile layout** with a slide-out menu.

</td>
</tr>
</table>

<br>

## Website
https://ltorl.github.io/openify-music/

<br>

## How it works

**`index.html`** is the whole app, with nothing to build or install. It plays music through the [Achroma](https://achromaon.top) music API and keeps your library and playlists in `localStorage`.

**`proxy.js`** is a tiny Cloudflare Worker that does one thing. It fetches the kworb global daily chart, turns it into JSON and adds CORS headers so the page can read it. It caches the chart for 30 minutes and serves the last good copy if kworb is down. Every other path returns 404.

```
GET https://free-kworb.anjomous.workers.dev/

{ "date": "2026-10-16", "rows": [ { "rank": 1, "move": "NEW", "id": "2sFepQNyl1WjpROruP8bJ0",
  "title": "Housewife", "artists": ["Tate McRae"], "feat": [], "days": 1, "peak": 1, "streams": 5671170 }, ... ] }
```

If the worker can't be reached, the page shows the last chart it loaded. If it has never loaded one, it shows the chart built into the file.

<details>
<summary><b>Hosting your own chart worker</b></summary>

<br>

Deploy `proxy.js` as a Cloudflare Worker:

```bash
npx wrangler deploy proxy.js --name my-kworb --compatibility-date 2026-10-01
```

You can also paste the file into a new Worker in the Cloudflare dashboard.

Then point the page at it by changing this line in `index.html`:

```js
const CHART='https://free-kworb.anjomous.workers.dev/';
```

The free Workers plan is plenty for this, since the worker hits kworb at most once every 30 minutes.

</details>

<br>

## Credits

Openify is a front end only. The music, data and services all come from these projects.

| What | Provided by |
| --- | --- |
| Player, search, streaming & lyrics | [Achroma](https://achromaon.top) |
| Songs | [YouTube Music](https://music.youtube.com) |
| Daily chart | [kworb.net](https://kworb.net), compiled from the [Spotify Charts](https://charts.spotify.com) |
| Chart worker hosting | [Cloudflare Workers](https://workers.cloudflare.com) |

Openify is not affiliated with Spotify, YouTube or Google.

<br>

## License

[GPL-3.0](LICENSE)
