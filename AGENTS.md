# Agent Guidelines for tuna-os/iced

`tuna-os/iced` is the upstream-derived cross-platform GUI library fork used throughout the tuna-os and COSMIC desktop ecosystems.

## Developer Workflows & Guidelines

- **Commit Signing**: All commits MUST be signed off using DCO (`git commit -s`).
- **Target Branches**: Submit pull requests against `master` unless instructed otherwise by maintainers or repository configuration.
- **Scope & Code Ownership**: Code changes to widget, runtime, renderer, or windowing logic must maintain compatibility with upstream iced APIs while supporting tuna-os desktop feature requirements.
- **Documentation**: Document public APIs, architectural choices, and feature additions clearly in Rustdoc comments and relevant markdown files.
