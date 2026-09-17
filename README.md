# WordStar Mini OTA artifacts

This repo holds nothing but published firmware updates for WordStar Mini
devices: `manifest.json` (the current version, its binary URL, and its MD5
checksum) plus one `.bin` per released version. It is intentionally public
and separate from the private firmware source in `plp-wordstar-mini` -
publishing a compiled binary here does not expose any source code.

Devices with home WiFi configured fetch `manifest.json` on every cold boot
(or on demand from the on-device WiFi panel's "Check for updates now"), and
if its `version` differs from what they're running, download and verify the
referenced `.bin` before installing it. See
`plp-wordstar-mini`'s `docs/firmware-setup-and-device-bringup.md` (section
10) for the full OTA design, including why plain HTTPS with no auth is an
acceptable tradeoff here (integrity is verified via the MD5 check, not TLS
trust), and how to publish a new release.

## Publishing a new release

1. In `plp-wordstar-mini`, bump `firmware/WordStarMini/version.h`'s
   `FIRMWARE_VERSION` and compile with the `default_8MB` FQBN.
2. Compute the MD5 of the resulting `WordStarMini.ino.bin` (not the merged
   image - just the application binary).
3. Add the renamed `.bin` (e.g. `WordStarMini-<version>.bin`) to this repo
   and update `manifest.json` to point at it with the new version and MD5.
4. Commit and push. Devices pick it up on their next cold boot or an
   on-demand check.
