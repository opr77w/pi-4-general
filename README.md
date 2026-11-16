# PI-4-GENERAL

General-purpose Raspberry Pi 4.

## Network and access

| | |
|---|---|
| Hostname | `pi-4-GENERAL` as set on the Pi (`pi-4-general.local` resolves; mDNS is case-insensitive) |
| IP | `192.168.7.229`, DHCP reservation |
| User | `pi`, key auth from the MacBook (copied 06/11/2026) |
| Fleet | in `PI-FLEET/hosts.json` as `pi-4-general` |

## Hardware and OS

Raspberry Pi 4 Model B Rev 1.2, 4 GB RAM. Root on a 32 GB SD card (29 GB ext4). Raspberry Pi OS 64-bit, Debian 13 Trixie, kernel 6.18.50+rpt-rpi-v8. Swap 2 GB.

## UPS (NUT)

NUT secondary (`nut/upsmon.conf`, the password is not in the repo) against `eaton@192.168.4.227` on pi-4-netutil. It shuts down cleanly when the primary signals.

## Open

- Hostname is `pi-4-GENERAL` (capitals) on the Pi.

## Notes

The BirdNET warm-standby role on this Pi (daily `birdnet-standby-sync` pulling from `pi-4-birdnet`, `birdnet-takeover`/`birdnet-handback` for failover) was retired after the 09/11/2026 cutover from `pi-4-birdnet` to `pi-5-birdnetgo`. The post-cutover BirdNET-Go upgrade means the DB may no longer open on the old build, so a Pi 4 rollback is no longer a supported path. The standby files have been removed from this repo; see the pre-removal commit for history.
