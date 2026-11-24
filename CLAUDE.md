# CLAUDE.md

Memory of the Trading Essentials repository (`https://github.com/star-racer/trading-essentials`): project
context, standing rules and current status. The global rules are in `~/.claude/CLAUDE.md` (versioned in
`primo-ideas/claude/CLAUDE.md`); technical documentation is in `docs/` (index: `docs/README.md`).

## What it is

Trading Essentials, a World of Warcraft addon (Retail, 12.1.5) that keeps a record of the player's auctions
and stock, with statistics: tabs for auctions, stock, stats and gold, item tooltip lines, a watch list,
quick selling at the auction house, Auctionator prices when present and a manual auction house scan
otherwise. Published on CurseForge: <https://www.curseforge.com/wow/addons/trading-book>.

## Layout

The addon is at the repository root (`TradingEssentials.toc`, the `.lua` files it lists, `UI/`, `Locales/`,
`Media/`); `install.ps1` copies only those into a local WoW install. `tests/` holds the test suite,
which runs the addon's logic on Lua 5.1 against a strict fake client. `art/` holds the icon sources.

## Build, test, release

```powershell
python -m pip install "lupa>=2.0"   # Python 3.12; lupa has no wheels for newer releases yet
python tests/run.py
.\install.ps1                        # then /reload in game
```

CI (`.github/workflows/ci.yml`, every push to `main`) runs the test suite on Lua 5.1; it deploys
nothing. Releases go through CurseForge (docs/development.md).

## Decisions

- 2026-11-17: the addon is renamed Trading Essentials, everywhere: folder and `.toc`
  (`TradingEssentials`), saved variables (`TradingEssentialsDB`), globals, `/tradingessentials`. No
  migration from Trading Book: its saved data was deleted, its tags removed, and versions restart at
  v1.0.0. The CurseForge slug is still `trading-book` until renamed there.
- 2026-11-17: the icon is a CC0 drawing of a pile of gold coins (OpenClipart), `art/icon.svg`.
- 2026-11-23: the GitHub repository moved to `star-racer/trading-essentials`; the old
  `primo-ideas/trading-book` is no longer the remote.
- 2026-11-24: the history is two commits: `License`, by the user (Joko Games, on purpose, not to be
  rewritten), then `Trading Essentials v1.0.0`, by Claude, which holds everything else. Commit
  messages carry no `Directed-by` trailer any more. The tag `v1.0.0` is local until the user publishes
  it (pushing it releases on CurseForge).

## Working rules

- **Language:** everything in the repository is in English, except translations (`Locales/`). Talk to
  the user in French.
- The logic is tested outside the game; what only the client can show is checked in game by the user
  (README, "Check in game").

## Status

- 2026-11-16: search in the Stats tab, recovery from a failed scan, order lists in the main settings
  page, a canvas settings page (to stop blocked actions), its own icon.
- 2026-11-17: this memory file and `docs/README.md` created; history rewritten so that every commit is
  by Claude, directed by Primo.
- 2026-11-17: renamed Trading Essentials, new pile-of-coins icon, released as v1.0.0.
- 2026-11-24: the Stock tab has a grid view (large icons, quantity in the bottom right corner), next to
  the list; the choice is saved in `TradingEssentialsDB.ui.stockView`.
- 2026-11-24 (decision): Stock is the first tab, the one the window opens on, and its grid is the
  default view. A saved tab order in the settings still wins.
- 2026-11-24 (decision): every item is counted, bound ones too, but kept apart (`stock.boundBags`,
  `stock.boundBank`, `warband.boundItems`). A "Show bound items" box in the Stock tab shows them there and in
  item tooltips (`ui.showBound`, off by default). They have no value and stay out of the history; the
  shopping list always counts them.
