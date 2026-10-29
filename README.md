# foobar2000-config

My foobar2000 v2.26 for Mac setup (`~/Library/foobar2000-v2/`).

![screenshot](docs/screenshot.png)

Components ([JendaT/fb2k-components-mac-suite](https://github.com/JendaT/fb2k-components-mac-suite), bundled):
Playlist Organizer, SimPlaylist, Waveform Seekbar.

## Restore

Quit foobar2000, then:

```sh
gh repo clone drugdamage/foobar2000-config ~/foobar2000-config-backup
rsync -a --exclude .git --exclude .gitignore --exclude README.md --exclude docs/ \
  ~/foobar2000-config-backup/ ~/Library/foobar2000-v2/
```

Music is expected in `~/dd/Songs`; elsewhere, re-add it in Preferences → Media Library.

## Update

```sh
rsync -a --delete --exclude .git --exclude .gitignore --exclude README.md --exclude docs/ \
  ~/Library/foobar2000-v2/ ~/foobar2000-config-backup/
cd ~/foobar2000-config-backup && git add -A && git commit -m "Update config" && git push
```
