# Spotify Controller

A compact Windows desktop app for controlling Spotify with global hotkeys, a clean "now playing" view, a system tray icon, a Twitch chat bot with song requests and OBS stream overlays.

> **Windows only.** The app uses Windows APIs (media keys, media session, `winsound`), so it does not run on macOS or Linux.

## Features

- **Global hotkeys** for Play/Pause, Next and Previous, freely configurable, work even when the window is hidden
- **Controller hotkeys**: skip, pause and like songs with your Xbox, PS4 or PS5 controller, including button combos (e.g. `LB + A`)
- **Like hotkey**: save the current song to your Spotify library with one key press (optional sound feedback)
- **Add to playlist**: pick a playlist in Settings → Spotify and add the playing song with one hotkey
- **Clear hotkey**: assign your own key to the Clear button
- **Twitch chat bot**: `!song` and `!songrequest` in your own channel, with viewer permissions and an on/off switch
- **Song request list**: see every request in its own window and click one to open it in Spotify
- **Now playing view** with cover art, progress bar and song history
- **3 now-playing designs**: Spotify, Spotlight, Clean
- **4 color themes**: Harbor, Nightfall, Moonlit, Steel, plus a custom wallpaper background
- **System tray**: minimize to tray, single-instance, optional `--tray` start
- **OBS overlays**: browser source for the current song (3 styles: Card, Minimal, Badge) and a separate one for song requests
- **Always on top** option

## Antivirus warning (false positive)

Some antivirus programs may flag `SpotifyController.exe` as a trojan (for example "Wacatac" or "Trojan.Python.Agent"). This is a **false positive**. It is a known issue with unsigned apps written in Python and packaged with PyInstaller, and with apps that use global hotkeys.

- The app does not download or run other programs and collects no personal data. It only connects to Spotify, Twitch (if you enable the chat bot), cover art services and GitHub (version check).
- You can check the file yourself on [VirusTotal](https://www.virustotal.com/) with this SHA256 hash: `8666f1a3dc7e35113f196b5a787b41ec56321b6baffdc2ea9b5a9c6e45f5ee82`[https://www.virustotal.com/gui/file/8666f1a3dc7e35113f196b5a787b41ec56321b6baffdc2ea9b5a9c6e45f5ee82?nocache=1]
- If your antivirus blocks it, add `SpotifyController.exe` to its exclusions list.
- I have reported the file to the antivirus vendors as a false positive.

If you are not comfortable running it, please don't.

## Requirements

- Windows 10 / 11
- Spotify desktop app
- Spotify Premium and an active Spotify player for song requests (Spotify only allows adding to the queue for Premium accounts)

## Download

1. Go to the [Releases](https://github.com/angelbyte00/Spotify-Hotkey-Controller/releases) page.
2. Download `SpotifyController.exe` from the latest release.
3. Run it. No installation needed.

**Updating:** Open **Settings → About** and click **Check for updates**. If a new version is available, the app opens the download page in your browser.

Start minimized to the tray:

```
SpotifyController.exe --tray
```

## Setup

### Spotify (Like, playlists, song requests)

1. Open the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) and create an app.
2. Add this Redirect URI: `http://127.0.0.1:8888/callback`
3. Copy the **Client ID** (and secret if requested) into **Settings → Spotify** and connect your account.
4. Assign keys to the Like and Add to Playlist actions in the **Hotkeys** tab.

The app requests these Spotify permissions: `user-library-modify` (Like), `user-read-playback-state` and `user-modify-playback-state` (song requests / queue), `playlist-read-private`, `playlist-modify-private` and `playlist-modify-public` (Add to Playlist).

If you update from an older version and a new feature does not work, disconnect and reconnect your Spotify account in **Settings → Spotify** so the new permissions are granted.

### Twitch chat bot

1. Create an app at [dev.twitch.tv/console](https://dev.twitch.tv/console) with **Client Type: Public** and the OAuth redirect URL `http://localhost`.
2. Paste the **Client ID** into **Settings → Twitch** and click **Log in with Twitch**.
3. Connect your Spotify account in **Settings → Spotify** (needed for song requests).

The bot only runs in your own channel, so there is no channel name to type and no token to copy.

| Command | Aliases | What it does |
|---|---|---|
| `!song` | `!currentsong`, `!nowplaying` | Posts the current song with its Spotify link |
| `!songrequest <song or Spotify link>` | `!sr`, `!request` | Adds the song to your Spotify queue |

- Choose who can request songs (Everyone / Subs & mods / Mods only) and switch song requests on or off with one click in **Settings → Twitch**.
- The bot writes under your own Twitch account and only needs the `chat:read` and `chat:edit` permissions.
- Twitch logins expire after 30 days and need a new login.

### OBS overlays

1. Enable the overlay in **Settings → Stream**.
2. In OBS, add a **Browser Source** with one of these URLs (the port can be changed in the settings):
   - Now playing: `http://127.0.0.1:8787/overlay`
   - Song requests: `http://127.0.0.1:8787/requests`

You can use both next to each other.

## Data and privacy

Everything stays on your PC. The app creates these files next to the program (they fall back to `%APPDATA%\SpotifyController` if the folder is read-only):

| File | Content |
|---|---|
| `settings.json` | Settings, your Spotify credentials / refresh token and your Twitch login |
| `hotkeys.json` | Your hotkey assignments |
| `controller.json` | Your controller button assignments |
| `history.json` | Recently played songs |
| `cover_cache/` | Cached cover images |
| `*.log` | Error and debug logs |

**Never share `settings.json`.** It contains your Spotify credentials and your Twitch login. When you give the program to someone else, only share `SpotifyController.exe`.

## Disclaimer

This is an unofficial fan project. It is not affiliated with, endorsed by or sponsored by Spotify or Twitch. All trademarks belong to their respective owners.

## License

Copyright (c) 2026 angelbyte00. All rights reserved. See [LICENSE](LICENSE).
