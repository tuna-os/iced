# AGENTS.md — Iced GUI Framework

**Iced** is a cross-platform GUI library for Rust inspired by Elm, providing a modern, declarative approach to building graphical user interfaces.

This document guides hives and agents exploring the iced repository.

## Repository Overview

- **Type**: Rust GUI framework/library
- **Platform**: Cross-platform (Linux, macOS, Windows)
- **Architecture**: Elm-inspired, functional reactive programming
- **Primary Language**: Rust (Edition 2021)
- **Key Dependencies**: Winit (windowing), Wgpu (graphics), Tokio (async)
- **License**: MIT

## Key Files and Structure

```
iced/
├── Cargo.toml                  # Workspace manifest
├── Cargo.lock                  # Locked dependencies
├── README.md                   # User documentation
├── CONTRIBUTING.md             # Contributor guidelines
├── DEPENDENCIES.md             # Dependency documentation
├── ROADMAP.md                  # Project roadmap
├── examples/                   # Example applications
│   ├── counter.rs             # Basic counter app
│   ├── todos.rs               # Todo list app
│   ├── styling.rs             # Theming examples
│   └── ...
├── src/                        # Library source
│   ├── lib.rs                 # Crate root
│   ├── widget/                # UI widgets
│   ├── element.rs             # Element abstraction
│   ├── application.rs         # Application framework
│   ├── theme.rs               # Theming system
│   └── ...
├── tests/                      # Integration tests
├── futures/                    # Async helpers crate
├── graphics/                   # Graphics crate
├── style/                      # Styling crate
└── web/                        # WebAssembly support (optional)
```

## Build and Development

### Prerequisites

- Rust toolchain (stable, edition 2021)
- Cargo (included with Rust)
- Native dependencies vary by platform:
  - **Linux**: GTK or X11 libraries + development headers
  - **macOS**: Xcode command line tools
  - **Windows**: MSVC or MinGW toolchain

### System Dependencies

On **Linux (Debian/Ubuntu)**:

```bash
sudo apt-get install libxkbcommon-dev libxkbcommon0 libxcb-render0-dev \
    libxcb-shape0-dev libxcb-xfixes0-dev libxcb-xkb-dev
```

On **Linux (Fedora)**:

```bash
sudo dnf install libxkbcommon-devel libxcb-devel libxkb-devel
```

### Build Commands

```bash
# Build the library
cargo build

# Build in release mode (optimized)
cargo build --release

# Build with specific features
cargo build --features wgpu,debug

# Run examples
cargo run --example counter
cargo run --example todos
```

### Running Examples

The repository includes many examples demonstrating different features:

```bash
# List all examples
ls examples/

# Run a specific example
cargo run --example counter
cargo run --example todos
cargo run --example styling

# Run with logging
RUST_LOG=debug cargo run --example counter
```

## Testing and Quality

### Running Tests

```bash
# Run all tests
cargo test

# Run tests for a specific crate (e.g., graphics)
cargo test -p iced_graphics

# Run tests with output
cargo test -- --nocapture

# Run doctests
cargo test --doc
```

### Code Quality

```bash
# Format code
cargo fmt

# Check formatting without modifying
cargo fmt -- --check

# Lint with clippy
cargo clippy --all-targets --all-features

# Check for security vulnerabilities
cargo audit
```

## Architecture and Design

Iced is organized as a workspace with multiple crates:

- **`iced`** — Main library (widgets, theming, application framework)
- **`iced_core`** — Core types (Element, Message, Hasher)
- **`iced_graphics`** — Graphics rendering abstractions
- **`iced_futures`** — Async/await integration and subscription support
- **`iced_native`** — Native widget implementations
- **`iced_wgpu`** — GPU rendering backend (Wgpu)
- **`iced_web`** — WebAssembly support (experimental)

### Elm-Inspired Model

Iced applications follow the Elm architecture:

1. **Model** — Application state
2. **View** — Render UI from state
3. **Update** — Handle messages and update state
4. **Subscription** — Respond to external events

## Feature Flags

Key Cargo features:

- `wgpu` — GPU rendering (default)
- `debug` — Debug utilities and tools
- `web` — WebAssembly support
- `tokio` — Tokio async runtime integration

Feature combinations affect:
- Available rendering backends
- Supported platforms
- Included widgets and styling options

See `Cargo.toml` for complete feature matrix.

## Dependency Management

### Adding Dependencies

1. Add to appropriate `Cargo.toml` (workspace or specific crate)
2. Use conservative versions: `exact_version = "1.2.3"` or `~1.2`
3. Justify in PR description
4. Test with CI matrix (different features/platforms)

### Updating Dependencies

```bash
# Check for outdated dependencies
cargo outdated

# Update specific crate
cargo update -p some-crate

# Update all dependencies (cautiously)
cargo update
```

## Platform-Specific Considerations

### Linux

- Different window managers and display servers (X11, Wayland)
- GL/Vulkan availability varies
- Test on both X11 and Wayland when possible

### macOS

- Requires Metal graphics backend (automatically handled)
- App sandboxing considerations for distribution

### Windows

- ANGLE or DirectX graphics backends
- High DPI support important

### WebAssembly

- Limited feature set (web crate)
- Browser canvas rendering
- No native OS integration

## Continuous Integration

CI runs on:
- **Linux** (X11 environment)
- **macOS**
- **Windows**
- **WebAssembly** (experimental builds)

Key checks:

- Format validation (`cargo fmt --check`)
- Clippy linting (`cargo clippy`)
- Tests (`cargo test`)
- Doc tests (`cargo test --doc`)
- Documentation building
- Example compilation

## Releasing

Release process:

1. Update version in workspace `Cargo.toml`
2. Update `CHANGELOG.md` with changes
3. Tag commit: `git tag vX.Y.Z`
4. Push tag to trigger CI release workflow
5. CI publishes to crates.io

Version follows semver (X.Y.Z).

## Development Workflow

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/name`
3. Make changes and test locally
4. Run format, lint, and tests
5. Commit with DCO sign-off: `git commit -s`
6. Push and open pull request
7. Ensure CI checks pass
8. Await review and merge

All commits must be signed off with the Developer Certificate of Origin (DCO).

## Common Tasks

### Adding a New Widget

1. Create new module in `src/widget/`
2. Implement `Widget` trait
3. Add to `lib.rs` module exports
4. Create example demonstrating usage
5. Add tests
6. Document public API with doc comments

### Adding a New Feature

1. Check `ROADMAP.md` for planned features
2. Discuss in issues if not already tracked
3. Implement feature behind feature flag if experimental
4. Add tests
5. Update documentation and examples
6. Update `ROADMAP.md` if completed

### Performance Optimization

1. Profile with `cargo flamegraph` or similar
2. Identify bottlenecks
3. Implement optimization
4. Benchmark before/after
5. Document changes

## Troubleshooting Build Issues

### Compilation Errors

```bash
# Clean build directory
cargo clean

# Rebuild with verbose output
cargo build --verbose

# Update dependencies to compatible versions
cargo update
```

### Missing Dependencies

Usually indicates system libraries not installed. Reinstall via package manager and verify with:

```bash
pkg-config --cflags --libs dependencies
```

### Platform-Specific Issues

- **X11/Wayland**: Check window manager and display server
- **Graphics**: Verify GPU driver and graphics API (GL/Vulkan/Metal)
- **Rendering**: Try different backend by adjusting features

## Related Documentation

- `README.md` — User guide and feature overview
- `CONTRIBUTING.md` — Contributor guidelines
- `DEPENDENCIES.md` — Dependency documentation
- `ROADMAP.md` — Planned features and direction
- [Iced Book](https://book.iced.rs/) — Official guide (external)
- [API Documentation](https://docs.rs/iced/) — Crate documentation (external)

## Community and Support

- **Issues**: File bugs and feature requests on GitHub
- **Discussions**: GitHub Discussions for ideas and questions
- **Examples**: Refer to `examples/` for patterns and usage
- **Documentation**: Consult `CONTRIBUTING.md` and inline doc comments
