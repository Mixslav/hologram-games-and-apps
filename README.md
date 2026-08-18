# Hologram — Games and Apps

Content for hologram v2.0

The device pulls from this repo on boot and mirrors it onto the SD card under
`/Hologram data/`. Folder names here must match the SD folder names exactly.

```
Animations/         .bin animation files
Apps and Games/     sealed .game apps
```

- **Animations** are only fetched when the card has none at all. Once the card
  has any animation, this folder is never read again — so deleting the stock
  ones on the device makes them stay deleted.
- **Apps and Games** are fetched per file: anything missing from the card, or
  whose contents differ from the copy here, is re-downloaded.

Content changes are detected by git blob hash, not file size, so **committing a
change here is all it takes** — the device picks it up on its next boot even if
the edit leaves the file exactly the same length. Nothing needs tagging or
releasing.

## Sealed apps

The `.game` files here are sealed containers, not readable Berry source: the
body is AES-256-CTR encrypted and the whole thing carries an ECDSA P-256
signature. Firmware verifies that signature before it will run a game, so a
`.game` this project didn't publish won't load.

They are still ordinary files as far as this repo is concerned — the sync
compares hashes and doesn't care what is inside them. Sealing is deterministic,
so re-sealing an unchanged app produces byte-identical output and does not
trigger a pointless re-download on every device.
