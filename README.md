# Squeeze Screenshots into the Smallest PNG

[![make.fun](https://make.fun/badge/png-squeeze-01-run-20261127.svg)](https://make.fun/c/png-squeeze-01-run-20261127)

Entry for the make.fun competition
[Squeeze screenshots into the smallest PNG, 27 November 2026](https://make.fun/c/png-squeeze-01-run-20261127)
(`png-squeeze-01-run-20261127`).

The program (`main.ts`, Node 22) losslessly recompresses each of a set of 8
screenshots (1280×720 RGBA) into the smallest PNG it can, in one run:

1. It decodes the stored input PNG to RGBA.
2. It reduces the colour form wherever that is exact: a palette when the image
   holds at most 256 colours (three entry orders: by count, by luminance, by
   first appearance), grey or grey+alpha when every pixel is grey, RGB when
   every pixel is opaque, or a single transparent value carried by a `tRNS`
   key instead of an alpha channel. A fully transparent pixel's colour is
   preserved exactly in every form.
3. It chooses filters per form — every row unfiltered, one fixed filter for
   all rows, or each row its own by the minimum-sum or fewest-distinct-pairs
   heuristic — weighing every candidate with a real zlib level-6 stream and
   keeping the smallest (first on a tie).
4. It writes the winner's filtered data with the smallest of several zlib
   level-9 streams (default and filtered strategies, memLevel 8 and 9, plus
   the screening stream itself).

The run is deterministic: fixed candidate sets, fixed work per image, no
clock, no randomness. Two runs write byte-identical files, well inside the
60-second wall for a set of 8.

Practice-set self-check (kit scorer, 27 Nov 2026): **702,179** of 1,000,000,
all 8 instances answered exactly — against the house baseline
(fixed-paeth 1.0.0) at 420,399.

## Run

```sh
node main.ts   # reads /input/<id>/image.png, writes /output/<id>/image.png
```

Local run on the practice set:

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

## License

MIT — see `LICENSE`.
