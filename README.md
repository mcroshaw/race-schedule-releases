# 🏁 Race Schedule – downloads

Shows when the next online races start for every series in **Le Mans Ultimate** (Race Control daily races) and **RaceRoom Ranked**, with live countdowns in your own time zone.

- One card per series: track, race length, cars, and the next start times
- Filter by level (Beginner / Intermediate / Advanced) or race type (Daily sprint / Daily feature / Weekly)
- RaceRoom: see how many drivers have signed up for each race, filter by region, and see which track the series moves to next
- Updates itself every 5 minutes

> **Unofficial fan tool.** It isn't made by or affiliated with Motorsport Games, Studio 397, Race Control or RaceRoom/KW Studios. It reads their public schedule pages and, for RaceRoom, the data the game client uses. If either site changes, the affected tab may stop working until a new version is released.

This repository only hosts the downloads. You don't need a GitHub account.

## Download

| Your computer | Download |
|---|---|
| Windows 10 / 11 | [RaceSchedule-windows.zip](https://github.com/mcroshaw/race-schedule-releases/releases/latest/download/RaceSchedule-windows.zip) |
| Mac with Apple chip (M1, M2, M3, M4…) | [RaceSchedule-mac-apple-silicon.zip](https://github.com/mcroshaw/race-schedule-releases/releases/latest/download/RaceSchedule-mac-apple-silicon.zip) |
| Older Intel Mac | [RaceSchedule-mac-intel.zip](https://github.com/mcroshaw/race-schedule-releases/releases/latest/download/RaceSchedule-mac-intel.zip) |
| Linux | [RaceSchedule-linux.zip](https://github.com/mcroshaw/race-schedule-releases/releases/latest/download/RaceSchedule-linux.zip) |
| Linux on ARM / Raspberry Pi | [RaceSchedule-linux-arm64.zip](https://github.com/mcroshaw/race-schedule-releases/releases/latest/download/RaceSchedule-linux-arm64.zip) |

These links always give you the newest version. Older versions are on the [Releases page](https://github.com/mcroshaw/race-schedule-releases/releases).

Not sure which Mac you have? Apple menu → **About This Mac** → look for "Chip: Apple…" (Apple chip) or "Processor: Intel".

There's nothing to install. The app runs a small page on your own computer and opens it in your browser. Keep its window open while you're using it; close the window to stop it.

### Windows

1. Right-click the downloaded zip → **Extract All…** → **Extract**.
2. Open the extracted folder and double-click **RaceSchedule.exe**.
3. The first time, Windows may show **"Windows protected your PC"**. This is because the app isn't code-signed (that costs money). Click **More info** → **Run anyway**.
4. A black window opens and your browser shows the schedule. Close the black window to stop it.

### Mac

1. Double-click the downloaded zip to unzip it (Safari often does this for you).
2. Double-click **RaceSchedule** in the unzipped folder.
3. The first time, macOS will say it **can't verify the developer** and won't open it. This is because the app isn't signed with a paid Apple developer account. To allow it:
   - Open **System Settings** → **Privacy & Security**, scroll down, and click **Open Anyway** next to the message about RaceSchedule. Confirm with your password or Touch ID.
   - Double-click **RaceSchedule** again and choose **Open**.
4. A Terminal window opens and your browser shows the schedule. Close the Terminal window to stop it.

*Shortcut for Terminal users:* `xattr -dr com.apple.quarantine ~/Downloads/RaceSchedule-mac-*` removes the block in one go.

### Linux

Unzip it, then run `./RaceSchedule` from the unzipped folder.

### Tips

- Running it again while it's already open just brings the page back up.
- Bookmark **http://localhost:4567** once it's running.
- Options: `RaceSchedule --port 8080` uses a different port; `--no-browser` doesn't open the browser.

## Hosting it on a server

Run it once on a server and everyone can just open a web address. Use the Linux download (or `-linux-arm64` on ARM):

```bash
unzip RaceSchedule-linux.zip
sudo mkdir -p /opt/race-schedule
sudo cp RaceSchedule-linux/RaceSchedule /opt/race-schedule/
sudo chmod +x /opt/race-schedule/RaceSchedule
sudo cp RaceSchedule-linux/race-schedule.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now race-schedule
```

The site is then at `http://<server-address>:4567`. In `--server` mode it listens on all network interfaces and never opens a browser. It fetches each schedule at most once every 3 minutes, however many people are viewing.

| Flag | Environment variable | Default |
|---|---|---|
| `--server` | `RACE_SCHEDULE_SERVER=1` | off |
| `--port 8080` | `PORT=8080` | `4567` |
| `--host 127.0.0.1` | `HOST=127.0.0.1` | `0.0.0.0` (all interfaces) |

To give it a proper web address with HTTPS, put it behind a reverse proxy such as [Caddy](https://caddyserver.com/) (`reverse_proxy localhost:4567`) and set `HOST=127.0.0.1`. `/healthz` returns `race-schedule-ok` for uptime checks.
