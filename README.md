# WordStar Mini - published updates and setup guide

This repo is intentionally public and separate from the private firmware
source in `plp-wordstar-mini`. It holds only what shipped devices fetch, and
the parent-facing setup guide:

| Path | What it is |
|---|---|
| `manifest.json` | Current firmware version, its `.bin` URL and MD5. Devices with home WiFi check it roughly every 12 hours (and on demand from the WiFi panel's "Check for updates now"). |
| `WordStarMini-<version>.bin` | One application image per released version. |
| `content/manifest.txt`, `content/phonics_list.csv`, `content/letter_order.txt`, `content/audio/*.pcm`, `content/pictures/*.pbm` | The phonics content: the sounds (mono clips with an 8-byte `WSPCM` header since 2026.09.19.4 - older firmware plays them wrongly, which is why devices update firmware before content), the programme list, and the letter order and letter-word pictures used by the letter checks. Devices on 2026.09.18.8+ fetch any file that is missing from their SD card or whose MD5 changed, once their firmware is current. |
| `setup/index.html` | The **setup guide** for families, served by GitHub Pages at <https://provident-learning-systems.github.io/plp-wordstar-mini-ota/setup/> - this is what the QR code in the box points to. Its footer shows which firmware it was written for and, live from `manifest.json`, the latest published firmware. |

Publishing a compiled binary or the audio here does not expose any source
code. Plain HTTPS with no auth is an acceptable tradeoff for this content:
devices verify every download against its MD5 before installing it (see
`plp-wordstar-mini`'s `docs/firmware-setup-and-device-bringup.md`, section
10, for the full design).

## Publishing

All of these are run from the `plp-wordstar-mini` checkout; each ends with a
commit + push here.

- **Firmware**: bump `firmware/WordStarMini/version.h`, compile with the
  `PartitionScheme=custom` FQBN, copy the resulting `WordStarMini.ino.bin` here
  as `WordStarMini-<version>.bin`, and point `manifest.json` at it with its MD5.
- **Sounds / phonics list**: `firmware/tools/publish_content.sh` regenerates
  `content/` and `content/manifest.txt`.
- **Setup guide**: edit `setup/index.html`. If a release changes anything the
  guide shows (button behaviour, screen layouts, panel wording), bump the
  guide revision and the "written for firmware" version in its footer so the
  two stay in step.
