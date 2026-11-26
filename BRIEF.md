# Brief: allnet-nvr-unchained

**Mode:** project
**Date:** 2026-11-26

## Goal

Generic, community-maintained Debian images for ALLNET NVR boards, so the hardware can run a current,
standard Linux instead of the vendor firmware. First board: ALL2289 (Rockchip RK3399). Next: ALL2268
(Allwinner A20). Unofficial project, not affiliated with ALLNET.

## Definition of done (milestone 1)

- README: what the project is, the unofficial/no-affiliation notice, supported boards, how to install,
  where releases live.
- Repository layout in place: `boards/<board>/` for board-specific parts, shared tooling at the top level.
- ALL2289 tooling ported and cleaned: image builder, bootloader build flow, U-Boot/TF-A patches, device
  tree, image overlay / first-boot setup, release test harness and its procedure.
- ALL2289 hardware facts and known limitations documented.
- Licensing in place (SPDX headers, `LICENSES/`).
- Every file checked by hand before commit: shellcheck where available, DTS compiles with `dtc`, and a
  grep for private or environment-specific details.
- Not required: a fresh image or bootloader build from this repo (only with explicit go-ahead).

## In scope / Out of scope

- In: docs, layout, licensing, ALL2289 tooling, a `boards/all2268/` placeholder with the known facts,
  and (last) public issues for the known ALL2289 limitations.
- Out: CI, ALL2268 bring-up, publishing an ALL2289 release (until the maintainer says so), upstream
  version upgrades during the port.

## Constraints & preferences

- Public repo, fresh history. Code is curated from a private development environment, never copied
  wholesale. Anything specific to that environment (build host, network, paths, device names, agent
  tooling) is left out or generalised into configuration.
- No secrets, personal details, LAN details or raw logs. No large binaries in git; releases carry them.
- Upstream versions pinned to tags, exactly as used for ALL2289 v1.0.0.
- Generic images: no user-specific names, keys or passwords; first-boot setup instead.
- Work on branches, one per step; each step is a PR that the maintainer merges. No branch protection.
- Releases per board: tags `<board>-vX.Y.Z` (e.g. `all2289-v1.1.0`), so boards can be released in phases.
- English for docs and commits.

## Confirmed assumptions

1. Licensing: GPL-2.0-or-later for device trees and U-Boot/TF-A/kernel patches, MIT for our own scripts
   and docs. SPDX header per file, full texts in `LICENSES/`.
2. Issue labels `board: all2289` / `board: all2268`, plus `bug`, `enhancement`, `docs`.
3. Known ALL2289 limitations become public issues with clean summaries, done last.
4. Board-specific names are generalised (e.g. a release file with a `BOARD=` field). Images released
   before this repo existed keep their old names; this is documented.
5. Test harness takes board address, serial device etc. from environment variables or a git-ignored
   local config file.
6. ALL2268 gets only a placeholder in milestone 1.

## Board facts known so far

| | ALL2289 | ALL2268 |
|---|---|---|
| SoC | Rockchip RK3399 (arm64) | Allwinner A20 (dual Cortex-A7, armv7 / armhf) |
| RAM | see `boards/all2289/` | 2 × 512 MB DDR3L (1 GB) |
| Boot chain | TF-A + U-Boot | U-Boot (no TF-A on A20) |

## Open questions

- ALL2268: boot medium, serial access, storage, network chip. Collected later, with a separate brief.
