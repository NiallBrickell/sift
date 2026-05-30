# sift

Sift through RAW+JPG photo shoots fast, the lazy way — no Lightroom import, no
waiting for a thousand RAWs to render.

## The idea

If you shoot **RAW+JPG**, every frame lands on disk twice: a slow-to-render RAW
(`.ARW`, `.CR3`, `.NEF`…) and the camera's already-baked JPG that opens
instantly. So:

1. **`sift open`** flicks every JPG up in a single Preview window, in shooting
   order. Delete the duds with `⌘⌫` as you go — it's quick, because Preview
   never has to decode a RAW.
2. **`sift tidy`** then bins every RAW whose JPG you just threw away, leaving you
   with RAWs only for the keepers.

That's the whole trick. The JPGs are disposable proxies; the RAWs are what you
take into your editor afterwards.

## Install

```sh
git clone https://github.com/NiallBrickell/sift.git ~/code/sift
ln -s ~/code/sift/sift ~/.local/bin/sift   # or anywhere on your $PATH
```

Make sure that bin dir is on your `PATH`. macOS only (it uses Preview and the
Finder Trash).

## Usage

```sh
sift open [dir]          # open every JPG in one Preview window (chrono order)
sift tidy [dir] [--dry]  # move every orphaned RAW (its JPG is gone) to Trash
sift tidy [dir] --dry    # just list what would go — change nothing
```

`dir` defaults to the current folder, so the usual run is just:

```sh
cd ~/Pictures/some-shoot
sift open
# …sift the JPGs in Preview, delete the duds, close it…
sift tidy --dry          # eyeball the list
sift tidy                # do it
```

## Safe by design

- `tidy` moves files to the **Trash** (via Finder), never `rm` — recoverable
  until you empty it.
- It always prints the list and a count and asks `y/N` first.
- `--dry` shows you exactly what would go without touching anything.
- It only ever deletes a RAW when **no** matching JPG exists; JPGs are never
  touched by `tidy`.

## RAW formats recognised

`arw cr2 cr3 nef nrw raf orf rw2 dng srw pef raw 3fr` — Sony, Canon, Nikon,
Fuji, Olympus, Panasonic, Pentax, Samsung, Adobe. Matching is
case-insensitive. Add your own to `RAW_EXTS` at the top of the script.

## Prior art / alternatives

This RAW+JPG sifting trick is well known; sift is just a tiny zero-dependency
take on it.

- **[FastRawViewer](https://www.fastrawviewer.com/)** (paid) — purpose-built for
  fast RAW browsing; reads embedded previews so RAWs open instantly and it can
  delete RAW+JPG pairs. The "proper" tool if you do this constantly.
- **Photo Mechanic** (paid) — the pro-standard fast photo browser.
- **Lightroom** — can do it, but importing and building previews for a thousand
  RAWs is exactly the slowness this avoids; better for editing the keepers
  afterwards.

If you just want to sift the fast JPGs with tools you already have and clean
up the RAWs after, that's sift.

## License

MIT — see [LICENSE](LICENSE).
