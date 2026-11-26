<p align="center">
 <img width="480" height="480" alt="icon" src="https://github.com/user-attachments/assets/9cbc56aa-c786-4936-8724-b17b32f69cee" />






# Spotify Controller

A compact Windows desktop app for controlling Spotify with global hotkeys, a clean "now playing" view with synced lyrics, a system tray icon, a Twitch chat bot with song requests and OBS stream overlays.

> **Windows only.** The app uses Windows APIs (media keys, media session, `winsound`), so it does not run on macOS or Linux.

## Videos

-  [Showcase](https://www.youtube.com/watch?v=LV511ti0lhU)
-  [Spotify Tab Setup (Like Hotkey, Playlist & Exact Matching)](https://www.youtube.com/watch?v=MhF1Db33KJY)
-  [Twitch Tab Setup (Chat Bot & Song Requests)](https://www.youtube.com/watch?v=nwztLMYJMHM)
-  [Discord](https://discord.gg/QW6yr9AH9H)

## Features

- **Global hotkeys** for Play/Pause, Next and Previous, freely configurable, work even when the window is hidden
- **Controller hotkeys**: skip, pause and like songs with your Xbox, PS4 or PS5 controller, including button combos (e.g. `LB + A`). Switch and other controllers work with the optional extended mode (SDL) in **Settings → General**
- **Like hotkey**: save the current song to your Spotify library with one key press (optional sound feedback)
- **Add to playlist**: pick a playlist in Settings → Spotify and add the playing song with one hotkey
- **Clear hotkey**: assign your own key to the Clear button
- **Twitch chat bot**: `!song` and `!songrequest` in your own channel, with viewer permissions and an on/off switch
- **Song request list**: see every request in its own window and click one to open it in Spotify
- **Now playing view** with cover art, seek bar, song history and synced lyrics (current line, via lrclib.net)
- **Spotify volume sync** once your Spotify account is connected
- **3 now-playing designs**: Spotify, Spotlight, Clean
- **Colors and background**: 4 color themes (Harbor, Nightfall, Moonlit, Steel), your own accent and background colors, and a custom wallpaper with adjustable dimming and blur
- **System tray**: minimize to tray, single-instance, optional `--tray` start, start with Windows
- **OBS overlays**: browser source for the current song in 8 styles (Banner, Banner Pro, Card, Badge, Player Bar, Vinyl, Polaroid, Angelcore) and a separate one for song requests
- **OBS song files**: optional `now_playing.txt` and `recent_songs.txt` for your own overlays
- **Performance mode** for slower computers (no animations, wallpaper or lyrics)
- **Always on top** option
- **In-app updates**: check for new versions and install them from the app

## Antivirus warning (false positive)

Some antivirus programs may flag `SpotifyController.exe` as a trojan (for example "Wacatac" or "Trojan.Python.Agent"). This is a **false positive**. It is a known issue with unsigned apps written in Python and packaged with PyInstaller, and with apps that use global hotkeys.

- The app does not run other programs and collects no personal data. It only connects to Spotify, Twitch (if you enable the chat bot), cover art services, lrclib.net (lyrics) and GitHub (version check and updates).
- You can check the file yourself on [VirusTotal](https://www.virustotal.com/gui/file/46ce75588fdfc0c18a12baa9d142c7cfa81070f9e7b9e860b981dd39aff08775). SHA256 hash: `46ce75588fdfc0c18a12baa9d142c7cfa81070f9e7b9e860b981dd39aff08775`
- To verify your download, run `Get-FileHash .\SpotifyController.exe -Algorithm SHA256` in PowerShell and compare the result with the hash above.
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

**Updating:** Open **Settings → About** and click **Check for updates** (or use the tray menu). If a new version is available, click **Update now** to download and install it. If the app cannot install it directly, it opens the download page instead.

Start minimized to the tray:

```
SpotifyController.exe --tray
```

## Setup

### Spotify (Like, playlists, volume, song requests)

> 🎬 Prefer a video? Watch [Tutorial 1: Spotify Tab Setup (Like Hotkey, Playlist & Exact Matching)](https://www.youtube.com/watch?v=MhF1Db33KJY).

1. Open the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) and create an app.
2. Add this Redirect URI: `http://127.0.0.1:8888/callback`
3. Copy the **Client ID** (and secret if requested) into **Settings → Spotify** and connect your account.
4. Assign keys to the Like and Add to Playlist actions in the **Hotkeys** tab.

The app requests these Spotify permissions: `user-library-modify` (Like), `user-read-playback-state` and `user-modify-playback-state` (song requests / queue, volume), `playlist-read-private`, `playlist-modify-private` and `playlist-modify-public` (Add to Playlist).

If you update from an older version and a new feature does not work, disconnect and reconnect your Spotify account in **Settings → Spotify** so the new permissions are granted.

### Twitch chat bot

> 🎬 Prefer a video? Watch [Tutorial 2: Twitch Tab Setup (Chat Bot & Song Requests)](https://www.youtube.com/watch?v=nwztLMYJMHM).

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

1. Enable the overlay in **Settings → Stream** and pick a style.
2. In OBS, add a **Browser Source** with one of these URLs (the port can be changed in the settings):
   - Now playing: `http://127.0.0.1:8787/overlay`
   - Song requests: `http://127.0.0.1:8787/requests` (add `?n=5` to choose how many entries are shown, 1 to 10)

You can use both next to each other.

## Data and privacy

Everything stays on your PC. The app creates these files next to the program (they fall back to `%APPDATA%\SpotifyController` if the folder is read-only):

| File | Content |
|---|---|
| `settings.json` | Settings, your Spotify credentials / refresh token and your Twitch login |
| `hotkeys.json` | Your hotkey assignments |
| `controller.json` | Your controller button assignments |
| `history.json` | Recently played songs |
| `now_playing.txt`, `recent_songs.txt` | Only if "OBS song files" is enabled |
| `*.log` | Error and debug logs |

**Never share `settings.json`.** It contains your Spotify credentials and your Twitch login. When you give the program to someone else, only share `SpotifyController.exe`.

## Disclaimer

This is an unofficial fan project. It is not affiliated with, endorsed by or sponsored by Spotify or Twitch. All trademarks belong to their respective owners.

## License

Copyright (c) 2026 R0bin-00. All rights reserved.
