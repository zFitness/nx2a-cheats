# nx2a-cheats

The cheat files the app imports: the community's [Atmosphère](https://atmosphere.re/) format codes, one file per
build, with the credit of the author beside them.

The app (nx2androidX) does not carry them — they are the community's files and they change when the community
changes them. The player downloads the package (a release asset, or the copy on the cloud drive) and imports it
in the app: **Settings → Environment → Cheats → Import zip**. The files then live in the app's own folder, and the
game's drawer lists the cheats of the build it runs.

## The layout

```
cheats/<title ID>/<build ID>.txt      the cheat file (the first 16 hex digits of main's build ID, upper case)
cheats/<title ID>/<build ID>.credit   who wrote it
```

A cheat file belongs to one build: it writes the game's memory at fixed addresses, so the file of another build
would write the wrong places. The app only uses the file whose name is the build it runs.

## What is in here, and what is not

- **The resolution patches are left out.** The app has its own render scale (the drawer's), and a community patch
  of the same setting would fight it — with both on, the result is not the one the player expects.
- **The frame rate patches are kept.** For the games where the app has no frame rate switch of its own, the
  community's patch is the only way to 60.
- Everything else is the community's file, byte for byte.

## Where the files come from

| Source | What it is |
|---|---|
| [switch-cheats-db](https://github.com/HamletDuFromage/switch-cheats-db) | the aggregator: one JSON per game, one section per build (the credit names the author) |
| [MyNXCheats](https://github.com/Arch9SK7/MyNXCheats) | one author's own collection (the files are already in the Atmosphère format) |
| [vgtxc/switch-cheat](https://github.com/vgtxc/switch-cheat) | the Chinese community's collection, the one the [NSboy](https://www.nsboy.net/) and [tekqart](https://tekqart.com/) forums share |

The first one that has a usable file wins; nothing is merged across them (two files that patch the same place
would fight, and the player would not know which one won).

## Adding a game

The files are pulled with the script in the app's repository:

```
python3 devtools/cheats_pull.py --repo <this repository> <title ID>:<build ID>
```

It takes the file from the first source that has it, leaves the resolution patches out and writes it here with
the credit. The build ID is the first 16 hex digits of the game's main build ID (the `BUILD_ID` line in the data
pack's `mods/main.fixups.tsv`).

## The package

The releases carry the whole thing as a zip, which is what the player imports:

```
python3 devtools/cheats_zip.py --repo <this repository>
```

The zip holds `cheats/<title ID>/<build ID>.txt` (and the credits); the app finds the files by their names, so
the layout of the community's own packages (`titles/<title ID>/cheats/<build ID>.txt`) is accepted too.

## Credits

The cheat files are the community's work — every file keeps the name of its author in the `.credit` file beside
it. This repository only collects them and leaves the resolution patches out; the authors keep their copyright.
