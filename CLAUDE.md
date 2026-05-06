# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Mean Tea is a single-file static HTML storefront for a tea/merch brand. Everything — HTML, CSS, and JavaScript — lives in `mean-tea.html`. There is no build step, no bundler, no package manager, and no test suite. Open the file directly in a browser to preview.

```
open mean-tea.html
```

## Architecture

### Single-file structure

The file is divided into three sections in order: `<style>` (inline CSS), `<body>` (HTML markup), `<script>` (inline JS). All are in one file — no external assets except the Geist Mono font loaded from Google Fonts.

### Page routing

Navigation is purely DOM-based. All pages (`#page-home`, `#page-shop`, `#page-about`, `#page-merch`, `#page-gang`, `#page-cart`, `#page-product`) are always present in the DOM. The `go(pg)` function removes `.active` from all `.page` elements and adds it to `#page-{pg}`. There is no URL routing.

### Themes (modes)

Three themes are controlled by a class on `<body>`:

| Class | Name | Notes |
|---|---|---|
| _(none)_ | Heaven | Light mode, default |
| `hell` | Hell | Dark mode |
| `shll` | Super Hell | Black + red; hides `.pages-wrap`, shows `.shll-screen` with canvas flame animation |

`setMode(m)` switches the body class and starts/stops the flame particle system via `startFlames()` / `stopFlames()`.

### Product data

All product data lives in the `PRODS` array (line ~488). Each entry has: `id`, `num`, `name`, `tag`, `desc`, `rat`, `reg`, `fmt`, `stk`, `price`, `type` (`"tea"` or `"merch"`). Tea IDs are `0–9`; merch IDs are `10+`. `showProd(idx)` looks up by `id` and populates the product detail page in-place.

### Cart

Cart state is a plain object `cart` keyed by product `id`, with shape `{ [id]: { p: product, q: quantity } }`. `updCnt()` re-counts and updates the badge. `renderCart()` rebuilds the cart DOM from scratch on every change.

### CSS conventions

Class names are heavily abbreviated (e.g., `.atcb` = add-to-cart button, `.mo` = mode option, `.pt` = product tile, `.ci` = cart item). All color and theme values are CSS custom properties on `body` — `--bg`, `--ink`, `--hi`, `--border`, `--soft`, `--hov`, `--bag`, `--lf`. Avoid hardcoding colors; use these variables.

### Flame canvas

The Super Hell flame effect is a 120-particle canvas animation (`startFlames()`). The canvas is sized to the `.shll-screen` element. Calling `stopFlames()` cancels the `requestAnimationFrame` loop.
