# updates

Source of the MicroPythonOS OTA update metadata, served at
https://updates.micropythonos.com from a VPS.

This repository is the **GitHub archive** of that data. Pushing here does not
deploy: the live server is updated by rsyncing a working copy with
`../upload_updates.sh` from the parent folder (one level up, outside this repo),
which also uploads the update logger and rewrites `micropythonos.com` to
`micropythonos.org` when given `--comtoorg`.

## Layout

- `osupdate_esp32.json`, `osupdate_esp32s3.json` and `osupdate_esp32p4.json` are
  the real metadata files. Every other `osupdate_<board>.json` is a symlink to
  one of them; keep it that way, the uploader preserves symlinks.
- `osupdate_unphone.json` is special because the unPhone 9 doesn't have enough
  storage space to support OTA updates, unless someone finds a solution.
- `osupdate_linux.json` is for linux releases for 0.4.1 and after. OTA is not
  available on desktop; it is a test fixture with a deliberately long changelog.
- Legacy device IDs still resolve through symlinks: `osupdate.json` is the
  waveshare_esp32_s3_touch_lcd_2 board and `osupdate_fri3d-2024.json` is the
  pre-0.4.1 `fri3d-2024` id.

## Releasing

A release adds `releases/<target>/MicroPythonOS_<target>_<version>.ota` (from
the GitHub release assets) and sets `version`, `download_url` and `changelog`
in the three real files. Do not do that by hand: in the MicroPythonOS checkout
run

```
python3 scripts/release_metadata.py <version>
```

which downloads the release assets and rewrites this repo and `../install`.
Then commit and push here and run `../upload_updates.sh`. The full procedure is
in the MicroPythonOS docs, `docs/other/release-checklist.md`.
