# PI-4-GENERAL

General-purpose Raspberry Pi 4, and BirdNET standby: backup for [pi-4-birdnet](../PI-4-BIRDNET/README.md). Added 06/11/2026.

## Network and access

| | |
|---|---|
| Hostname | `pi-4-GENERAL` as set on the Pi (`pi-4-general.local` resolves; mDNS is case-insensitive) |
| IP | `192.168.7.229`, DHCP reservation |
| User | `pi`, key auth from the MacBook (copied 06/11/2026) |
| Fleet | in `PI-FLEET/hosts.json` as `pi-4-general` |

## Hardware and OS

Raspberry Pi 4 Model B Rev 1.2, 4 GB RAM. Root on a 32 GB SD card (29 GB ext4). Raspberry Pi OS 64-bit, Debian 13 Trixie, kernel 6.18.50+rpt-rpi-v8. Swap 2 GB. Checked via pi-fleet 06/11/2026: no throttling, no failed units.

## Roles

- General purpose.
- **Warm standby for pi-4-birdnet** (set up 06/11/2026). BirdNET-Go is installed but the service is disabled; it only runs during a takeover.

## BirdNET warm standby

| | |
|---|---|
| Image | `ghcr.io/tphakala/birdnet-go@sha256:4dec04b2…290a` (same digest as pi-4-birdnet, built 14/10/2026), tagged locally as `:standby` |
| Unit | `systemd/birdnet-go.service`, installed **disabled**. Data on SD under `/srv/birdnet-go-app` |
| Sync | `birdnet-standby-sync`, daily 12:30 (after pi-4-birdnet's 11:45 DB snapshot). Pulls `config/` and `db-snapshot/birdnet.db`, integrity-checked. Skips while BirdNET-Go runs here |
| Sync access | key `~/.ssh/birdnet_sync_ed25519`, allowed on pi-4-birdnet only as `rrsync -ro /srv/birdnet-go-app/` (read-only, no shell) |
| Alerts | `pi-notify` (HA webhook) on sync failure, stale snapshot (>36h) and on takeover |
| Mic | none here; the USB card (C-Media 0d8c:0014) and Clippy move across on takeover |

### Takeover

1. Stop BirdNET-Go on pi-4-birdnet if it is up: `ssh pi@192.168.4.37 'sudo systemctl stop birdnet-go'`.
2. Move the USB sound card and Clippy to pi-4-general.
3. `sudo birdnet-takeover` on pi-4-general. It refuses if pi-4-birdnet's UI still answers (override with `--force`) or if the card is missing. It then does a final sync, promotes the snapshot DB to live, starts the service and checks the UI and capture.
4. Repoint by hand to `192.168.7.229`: pi-0-frame's Fugleramme BirdNET-Go URL, plus HA cards and links to `:8080`. MQTT sensors follow automatically.

Clips go to the SD card during a takeover (`/srv/birdnet-go-app/external/clips`).

### Handback

`sudo birdnet-handback` on pi-4-general stops the service and writes a consistent DB copy to `/srv/birdnet-go-app/handback/birdnet.db`. It then prints the steps to move the card back, copy the DB to pi-4-birdnet from the Mac (`scp -3`), restart it there and repoint.

### Verified 06/11/2026

- The sync pulled 3,342 detections with integrity ok.
- Dry run on port 8081 with MQTT, BirdWeather and push disabled: the UI returned 302 and the API served the synced detections. The container was stopped afterwards and `birdnet-go` stayed disabled.
- The scratch copy is at `/srv/birdnet-go-dryrun` on the Pi, and is safe to remove.

## UPS (NUT)

NUT secondary (`nut/upsmon.conf`, the password is not in the repo) against `eaton@192.168.4.227` on pi-4-netutil, which is the same as pi-4-birdnet. It shuts down cleanly when the primary signals. Verified 06/11/2026: `upsc` reads status OL at 100%, and the primary lists 192.168.7.229 as a client.

## Open

- Kernel 6.18.50 here against 6.18.60 on pi-4-birdnet; align them before relying on the standby.
- Hostname is `pi-4-GENERAL` (capitals) on the Pi.
