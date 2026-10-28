# foobar2000 for Mac – configuration backup

Backup of `~/Library/foobar2000-v2/` from **foobar2000 v2.26** (macOS).

## Installed components

From [JendaT/fb2k-components-mac-suite](https://github.com/JendaT/fb2k-components-mac-suite):

| Component | Folder | Version |
|---|---|---|
| Playlist Organizer | `user-components/foo_jl_plorg/` | 1.5.0 |
| SimPlaylist | `user-components/foo_jl_simplaylist/` | 1.6.0 |
| Waveform Seekbar | `user-components/foo_jl_wave_seekbar/` | 1.2.0 |

The component bundles are included in this repo, so a restore does not require re-downloading them.
To update them, download newer releases from the repository above.

## What is in here

| Path | Contents |
|---|---|
| `config.sqlite` | Main preferences (core + component settings, column layouts, volume) |
| `UI-Layout.txt` | UI layout (waveform seekbar, playlist organizer tabs, SimPlaylist, album art, controls) |
| `foo_plorg.yaml`, `plorg_volume_uuids.json` | Playlist Organizer tree and settings |
| `playlists-v2.0/` | Playlists |
| `library-v2.0/` | Media library database |
| `metadb.sqlite` | Track metadata database |
| `foo_out_upnp-config-v2.txt` | UPnP output device settings |
| `LargeFieldsConfig-v2.txt`, `version.txt` | Misc config / version marker |
| `dsp-presets/` | DSP presets (empty folders are not tracked by git) |
| `user-components/` | Third-party components (see above) |

Not included (see `.gitignore`): `waveform_cache/`, `ImageCache/` and other caches, logs, crash reports,
temporary/runtime files (`running`, SQLite `-wal`/`-shm` files), and anything over 50 MB.
foobar2000 and the components recreate these automatically.

## Restore

1. **Quit foobar2000** completely (⌘Q).
2. Keep a copy of the current config, just in case:
   ```sh
   mv ~/Library/foobar2000-v2 ~/Library/foobar2000-v2.old
   ```
3. Clone this repository:
   ```sh
   gh repo clone foobar2000-config ~/foobar2000-config-backup
   # or: git clone https://github.com/<your-user>/foobar2000-config.git ~/foobar2000-config-backup
   ```
4. Copy the config into place (without the git metadata and repo files):
   ```sh
   mkdir -p ~/Library/foobar2000-v2
   rsync -a --exclude .git --exclude .gitignore --exclude README.md \
     ~/foobar2000-config-backup/ ~/Library/foobar2000-v2/
   mkdir -p ~/Library/foobar2000-v2/dsp-presets
   ```
5. Start foobar2000. Check **Preferences → Components** to confirm the three components loaded.

Notes:
- The library and playlists reference music under `/Users/webnoise/Music` and `/Users/webnoise/Downloads`.
  On a machine with a different username or music location, re-add the music folders in
  **Preferences → Media Library** and rescan.
- The waveform cache is rebuilt as tracks are played.

## Updating this backup

```sh
rsync -a --delete --exclude .git --exclude .gitignore --exclude README.md \
  ~/Library/foobar2000-v2/ ~/foobar2000-config-backup/
cd ~/foobar2000-config-backup && git add -A && git commit -m "Update config" && git push
```

Quit foobar2000 first so the SQLite databases are in a consistent state.
