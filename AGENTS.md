# AGENTS.md — agent guide for tuna-os/iced

A **fork of the iced GUI library**, maintained by TunaOS to enable
cross-platform desktop application development in Rust. This repository tracks
upstream changes and applies TunaOS-specific patches.

Human docs: [`README.md`](README.md) (overview, examples), [`CONTRIBUTING.md`](CONTRIBUTING.md)
(upstream contribution guidelines), [`DEPENDENCIES.md`](DEPENDENCIES.md).

## Key Facts Before You Start

1. **This is a fork of upstream iced.** The canonical repository is
   [iced-rs/iced](https://github.com/iced-rs/iced). The TunaOS fork applies
   patches specific to TunaOS projects and may diverge from upstream on
   release cadence or feature prioritization.

2. **Default branch is `master`.** All work targets `master`. When syncing with
   upstream, merge upstream `master` into the local `master` branch.

3. **Upstream contribution process applies.** For changes that would be useful
   upstream, open issues and PRs on the canonical
   [iced-rs/iced](https://github.com/iced-rs/iced) repository first. The core
   team requires community feedback in
   [the Discourse forum](https://discourse.iced.rs/) before accepting code.

4. **Rebase workflow for TunaOS-specific changes.** If a change is specific to
   TunaOS (e.g., a COSMIC desktop integration, TunaOS-specific widget), keep it
   in a separate branch and rebase it on top of each upstream merge to minimize
   conflicts.

## Layout & Key Files

| Path | Purpose |
|------|---------|
| `src/` | Core iced library source (renderer-agnostic) |
| `wgpu/`, `tiny_skia/`, `winit/` | Renderer and windowing modules |
| `examples/` | Example applications demonstrating iced features |
| `Cargo.toml` | Workspace root with all crates |
| `ROADMAP.md` | Upstream roadmap and experimental features |

## Build & Test

The workspace uses Cargo and includes multiple crates (renderers, examples):

```bash
# Build all crates
cargo build --workspace

# Run tests
cargo test --workspace

# Build examples
cargo build --examples

# Run a specific example
cargo run --example counter
```

**Note:** Some examples may require a display server (X11 or Wayland). Headless
CI runs tests without display support; some features cannot be fully exercised
in that environment.

## Syncing with Upstream

When pulling in upstream changes:

1. **Fetch upstream:**
   ```bash
   git remote add upstream https://github.com/iced-rs/iced.git 2>/dev/null || true
   git fetch upstream master
   ```

2. **Create a sync branch:**
   ```bash
   git checkout -b sync/upstream-master upstream/master
   ```

3. **Merge into local master (or rebase TunaOS changes):**
   ```bash
   git checkout master
   git merge sync/upstream-master
   ```

4. **Resolve conflicts if needed,** test the build, and push.

## TunaOS-Specific Patches

If your change is specific to TunaOS (not useful upstream), document it clearly
in the commit message and PR. Ideally, keep such patches isolated in dedicated
commits or branches so they can be easily rebased during upstream syncs.

Examples of TunaOS-specific changes:
- COSMIC desktop integration or theming
- TunaOS branding or widget customizations
- Flatpak-specific paths or behaviors

## CI & Workflows

The repository runs Rust checks and tests on each push and pull request:
- Build verification
- Unit tests (with coverage reporting)
- Linting (cargo clippy)

All checks must pass before merging.

## Versioning & Releases

The TunaOS fork follows upstream version numbering where possible. Releases are
tagged when significant changes land; consult [`CHANGELOG.md`](CHANGELOG.md) for
version history.

For crates.io publication, consult the upstream iced project — the TunaOS fork
is primarily consumed via git dependencies in Cargo.toml from TunaOS projects.

## Getting Help

- **Upstream documentation:** https://docs.rs/iced/
- **Examples:** See `examples/` directory in this repository
- **Community:** [Discourse forum](https://discourse.iced.rs/) and Discord
- **Issues:** File bugs or feature requests on this repository for TunaOS-specific issues, or on the canonical [iced-rs/iced](https://github.com/iced-rs/iced) for upstream concerns
