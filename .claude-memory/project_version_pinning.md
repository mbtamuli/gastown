---
name: Gastown/Beads version pinning history
description: gt and bd are pinned to specific versions built from source to prevent auto-upgrade breakage; stale binary check disabled in code
type: project
---

gt and bd are built from source and pinned to prevent auto-upgrade instability.

**Current versions (as of 2026-03-13):**
- gt v0.12.0 at `~/.go/bin/gt`
- bd v0.60.0 at `~/.go/bin/bd`
- dolt v1.83.5 via Homebrew

**History:**
- 2026-03-11: Gastown auto-upgraded itself without permission, causing instability. Initially pinned to v0.10.0/v0.57.0 built from source on a `pin/v0.10.0` branch, installed to `~/.local/bin/`.
- 2026-03-12: Stale binary check (`root.go`) was commented out in code since `GT_STALE_WARNED=1` only works in interactive shell, not tmux agent sessions.
- 2026-03-12: Upgraded to v0.12.0/v0.60.0 (steveyegge Discord announcement) using `go install`, which placed binaries in `~/.go/bin/` (GOBIN). Same pinning approach with stale check disabled.
- 2026-03-13: Old `pin/v0.10.0` branch was gone. Created new `pin/v0.12.0` branch on fork (mbtamuli/gastown) disabling both the go-build warning and stale binary check in `internal/cmd/root.go`.

**Key details:**
- `HOMEBREW_NO_AUTO_UPDATE=1` and `GT_STALE_WARNED=1` set in `~/.zshrc`
- Homebrew gastown/beads packages were uninstalled; only dolt remains on Homebrew
- The `pin/` branch in the gastown fork carries two patches: BeadsInstallPath pin + stale check disable

**Why:** Gastown's auto-upgrade behavior broke the system. Pinning to known-good versions prevents surprise breakage.

**How to apply:** When upgrading gt/bd, use `go install` from the `pin/v0.12.0` branch on fork (mbtamuli/gastown) which has the warning/stale-check patches. Binaries land in `~/.go/bin/`. For new versions, create a new `pin/vX.Y.Z` branch, re-apply the patches to `internal/cmd/root.go`, and `go install ./...`. Never let Homebrew or auto-update manage these binaries.
