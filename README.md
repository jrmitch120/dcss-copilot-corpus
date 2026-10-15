# dcss-copilot-corpus

Recorded Dungeon Crawl Stone Soup games, kept as release assets, that [DCSS Copilot](https://github.com/jrmitch120/dcss-copilot)'s tests and evals replay.

The games live in **releases**, not in git: one release per Crawl Version, tagged `crawl-<version>` (such as `crawl-0.34.1`), holding that version's Recordings (`*.dcssrec`). The main repo lists every game in [`crates/replay/corpus.toml`](https://github.com/jrmitch120/dcss-copilot/blob/main/crates/replay/corpus.toml), pinned by SHA-256, so a changed or replaced asset is refused rather than replayed.

## Using it

From the main repo:

```sh
cargo run -p corpus -- fetch                            # downloads what isn't cached yet, checking each hash
cargo test -p copilot-replay --test corpus -- --ignored # replays every game on its tagged Crawl Version
```

## Adding a game

Record it on local webtiles with `tools/corpus` (see [`docs/corpus.md`](https://github.com/jrmitch120/dcss-copilot/blob/main/docs/corpus.md)), upload it to its version's release, and commit the manifest entry the recorder prints to the main repo:

```sh
gh release create crawl-0.34.1 --repo jrmitch120/dcss-copilot-corpus --title "Crawl 0.34.1" --notes ""   # once per version
gh release upload crawl-0.34.1 --repo jrmitch120/dcss-copilot-corpus qw-0.34.1-seed-7.dcssrec
```

Never replace an asset in place: a game that changes is a new file with a new hash.

## What a Recording holds

A Recording is everything the webtiles server sent during a game, as zstd-compressed JSON Lines. That includes account and character names and any chat, so only upload games whose players are happy for them to be public.

## Licence

GPL-3.0-or-later, as the main repo. The Recordings contain Dungeon Crawl Stone Soup's own game text, which is GPL-licensed by the crawl developers.
