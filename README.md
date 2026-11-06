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
- BirdNET standby for pi-4-birdnet. Scope to be decided.

## Open

- Decide what "BirdNET backup" means: cold spare, warm standby with BirdNET-Go installed, or a backup target for its data.
