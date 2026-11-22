# HANDOFF — lsd-decomp

**Last updated: 2026-11-22 (America/New_York).**
**Repo:** `mhvnsnt/lsd-decomp` (PUBLIC), default branch `main`.
**Purpose of this file:** any agent that picks up work here reads this FIRST, then the linked docs. It states what we've done, where we're at, and what we're working toward — so the owner can say "go to this repo, pick up the work" and you just know. Update this file in the same PR whenever the state changes (add, don't delete).

## 1. What this repo is

A matching decompilation of **LSD: Dream Emulator** (PlayStation, Asmik Ace, 1998), retail release SLPS-01556 — mirrored into the owner's account 2026-11-20. Every function of the game is C that rebuilds the retail executable byte for byte. The point is to understand how the game works: the dream graph, links, special days, the Grey Man, texture sets, the random seed. Each finding is written for players, with the function and address that prove it.

This is the actively documented one of the owner's LSD decomps. It is NOT the same as the others — see §5 and the related repos below. How they relate:
- `mhvnsnt/lsddecomp` — the brian-oblivion lineage complete matching decomp (CC0). Also complete; different codebase and docs.
- `mhvnsnt/lsd_decompiled` — the cawfee lineage, early-stage work-in-progress decomp.
- `mhvnsnt/lsddecomp-1` — the FirecatFG first attempt from 2024, assumption-heavy, last touched 2024-09-13.
- `mhvnsnt/LSDRevamped` — Figglewatts' Unity remake of the game (not a decomp).
- `mhvnsnt/libLSD` — Figglewatts' C# library for loading LSD data files.

## 2. Where we're at

### DONE
- **1,447 of 1,447 game functions in C**, compiled by GCC 2.6.3 and ASPSX 2.21 at -O2 -G8; `make check` compares the sha1 with the retail `SLPS_015.56`. (README, verified state at mirror time.)
- **1,903 names of our own**, each with evidence in `config/symbol_addrs.txt`. An audit rated all 4,159 names: 192 checked in game, 3,899 proven by the code, 68 guesses that say what is unproven (`config/name_levels.tsv`).
- **68 source files** laid out the way the original programmers most likely had them, in link order between Sony's objects (`docs/layout.md`).
- **Every function read:** no function body uses a placeholder name any more. What the game never uses is named as such (`unused4C`, `App_EmptySlot48`), with proof nothing reads or calls it.
- **56 findings and 4 community myths checked** in `docs/discoveries.md`: 35 checked in an emulator, 12 partly, 13 read in the code only. Highlights: the secret video reel (face on the graph → 11 min 13 s of special-day videos), the Grey Man flipping the graph and wiping memories, SELECT + TRIANGLE in pause erasing the whole game, the first dream of a new year starting from the wrong data, red-day rainbow penguins, a Clockwork Machines track that can never play.
- **Shiftable:** every pointer is a symbol; a build with 0x100 extra bytes before `main` played the same dream as retail in an emulator.
- **Recent work (2026-11-13 → 2026-11-15):** progress map ("every function read"), README for first-time readers, design-intent page, questions 10, 11, 15 closed. Last push 2026-11-15 (`65efa28`).
- **Mirrored into this account 2026-11-20** (repo created here that day; history carried over).

### IN PROGRESS
- Nothing open on this repo itself. No open PRs, no releases.

### BLOCKED
- **Question 12 (mysteries):** `docs/mysteries.md` — 14 of 15 questions answered, 1 still partly open (question 12). The repo itself does not say what would close it; read the mysteries doc before touching it.

## 3. What we're working toward

Understanding, not just matching. The executable already rebuilds byte-for-byte; the work now is reading: close question 12, keep the findings honest (emulator-checked where claimed), and keep every name proven or labeled a guess. For the owner's Sato games work (Western Heart pulls LSD in as a main Eastern Mind pair), this repo is the reference for how LSD actually works — the dream graph, links, special days, the Grey Man. Pull findings from here; do not re-derive them.

## 4. Standing rules that apply here

- **Branch → PR → merge, always.** Never push to main directly. Add-only, zero-conflict PRs, no force pushes. (This is a public mirror of a matching decomp — a bad merge breaks the byte match for everyone.)
- **Checkpoint-and-save law:** CHECKPOINT.md first, update after every step; the repo is the save; resume, never restart.
- **No-temp-storage law:** nothing in /tmp or any wipeable location, ever. Everything goes straight into the repo or a durable folder the second it exists.
- **Real-source-only:** this whole repo IS the real source. A change is kept only when `make check` still matches — that is the repo's own rule, stated in its README ("How it was made").
- **Licence/source logged honestly:** fan project, not affiliated with Asmik Ace or Osamu Sato. It ships no game asset: no executable, no disc data, no audio, no dump of the game's text or images. You need your own copy of the game to build anything. A takedown request from the rights holder will be honored.
- **Never kill in-progress work.** Never delete game files or other lanes' work without the standing verification rules.

## 5. Key files and paths

| What | Where |
|---|---|
| Findings, for players first | `docs/discoveries.md` |
| The 15 questions (14 answered) | `docs/mysteries.md` |
| Design intent | `docs/design-intent.md` |
| How each system works | `docs/engine/` |
| Naming conventions + glossary | `docs/naming.md` |
| Source layout in link order | `docs/layout.md` |
| Progress map (one rect per function) | `docs/progress/treemap.svg`, `docs/progress/progress.json` |
| Game code | `src/` (68 files), `include/game/`, `config/symbol_addrs.txt`, `config/name_levels.tsv` |
| Build + verify | `Makefile`, `tools/setup.sh`, `make check-disk`, `make split && make check` (must print `build/SLPS_015.56: OK`) |
| Related repos (NOT this one) | `mhvnsnt/lsddecomp`, `mhvnsnt/lsd_decompiled`, `mhvnsnt/lsddecomp-1`, `mhvnsnt/LSDRevamped`, `mhvnsnt/libLSD` |

## 6. Open decisions needing the owner

- **Which LSD decomp is the one to build on?** This repo (documented, 1,447/1,447 read) vs `lsddecomp` (brian-oblivion, also complete). Both are complete matching decomps; the docs and the naming differ. The Sato-games pull into Western Heart needs one canonical source — his call, not a lane's.
- **Question 12:** whether to keep pushing on the last partly-open mystery. His call on effort.

## 7. How to pick up

1. Read this HANDOFF.md first, then `README.md` and `docs/discoveries.md`.
2. To verify the build you need your own dump of the game disc (SLPS-01556) plus a Japanese PS1 BIOS for in-game checks — neither is in the repo, by design. `tools/setup.sh`, then `make check-disk`, then `make split && make check`.
3. Never commit game data, BIOS, disc images, or audio. Never break the byte match — if `make check` fails, the change does not land.
4. Land work branch → PR → merge, add-only.
5. Update this HANDOFF.md in the same PR when the state changes.
