# Iced Architecture Overview

Iced is a modular, cross-platform GUI library for Rust inspired by The Elm Architecture. It decomposes interface development into type-safe, reactive primitives and delegates windowing and rendering to modular workspace crates.

## Workspace Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                       iced (Facade)                         │
└──────────────────────────────┬──────────────────────────────┘
                               │
       ┌───────────────────────┼───────────────────────┐
       ▼                       ▼                       ▼
┌──────────────┐       ┌──────────────┐        ┌──────────────┐
│ iced_widget  │       │ iced_runtime │        │ iced_futures │
└──────┬───────┘       └──────┬───────┘        └──────────────┘
       │                      │
       ▼                      ▼
┌──────────────┐       ┌──────────────┐
│  iced_core   │       │  iced_winit  │ (Windowing Shell)
└──────┬───────┘       └──────┬───────┘
       │                      │
       └──────────┬───────────┘
                  ▼
       ┌──────────────────────┐
       │    iced_graphics     │
       └──────────┬───────────┘
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
┌──────────────┐    ┌─────────────────┐
│  iced_wgpu   │    │ iced_tiny_skia  │
└──────────────┘    └─────────────────┘
```

## Crate Responsibilities

### Core Primitives
- **`iced_core`**: Foundation traits (`Widget`, `Element`, `Renderer`, `Layout`, `Hasher`), geometry primitives (`Point`, `Size`, `Rectangle`), color, and theme contracts.
- **`iced_futures`**: Async runtime integration (`Subscription`, `Command`, futures executor bridge).

### Application & Runtime
- **`iced_runtime`**: Renderer-agnostic reactive runtime. Implements state management, message passing, event loops, and layout tree construction.
- **`iced_widget`**: Standard built-in widget collection (`button`, `column`, `row`, `text`, `scrollable`, `text_input`, etc.) implementing `Widget<Message, Theme, Renderer>`.

### Windowing & Shell
- **`iced_winit`**: Winit integration mapping OS window events, cursor movements, keyboard input, and touch events into Iced events.

### Rendering Engine
- **`iced_graphics`**: Base rendering abstraction layer over vector primitives, meshes, text pipelines, and image caching.
- **`iced_wgpu`**: Hardware-accelerated GPU renderer leveraging `wgpu` (Vulkan, Metal, DX12, WebGPU).
- **`iced_tiny_skia`**: Pure-software rasterization fallback renderer using `tiny-skia`.

## Execution Lifecycle

1. **User Event**: OS inputs are received via `iced_winit` and translated to `iced_core::Event`.
2. **Update Stage**: Messages produced by widgets or async subscriptions invoke `Application::update(&mut self, message)`.
3. **View Stage**: `Application::view(&self)` generates a declarative widget tree `Element<Message, Theme, Renderer>`.
4. **Layout Stage**: `iced_runtime` calculates node positions and bounds via `Widget::layout`.
5. **Draw Stage**: `Widget::draw` emits draw primitives to `iced_wgpu` or `iced_tiny_skia` for rendering.
