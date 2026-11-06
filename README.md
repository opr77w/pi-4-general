# PI-4-GENERAL

General-purpose Raspberry Pi 4, and BirdNET standby: backup for [pi-4-birdnet](../PI-4-BIRDNET/README.md). Added 06/11/2026.

## Network and access

| | |
|---|---|
| Hostname | `pi-4-general` (`pi-4-general.local`) |
| IP | `192.168.7.229`, DHCP reservation |
| User | `pi`, key auth from the MacBook (to be set up) |
| Fleet | in `PI-FLEET/hosts.json` as `pi-4-general` |

## Roles

- General purpose.
- BirdNET standby for pi-4-birdnet. Scope to be decided.

## Open

- Copy the Mac's SSH key to the Pi, then confirm `pi_info` via pi-fleet.
- Record model, RAM, boot media and OS once reachable.
- Decide what "BirdNET backup" means: cold spare, warm standby with BirdNET-Go installed, or a backup target for its data.
