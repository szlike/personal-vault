# Webpack / Rollup Explainer

**Recorded:** 2026-10-05

## Context

Explainer session on Webpack and Rollup, October 5, 2026. Webpack is a bundler that takes many JavaScript modules and their assets and packs them into files a browser can load. Rollup is a bundler that focuses on producing small, clean output, and is often used for libraries.

## The mechanism, spelled out

In development, a tool like Hot Module Replacement (the feature that swaps a changed module in a running page without a full reload) lets you edit code and see the change without rebuilding everything by hand. In production, the code is still bundled, and Rollup is the tool doing that bundling step. The production output is a small set of files built ahead of time, not the same live-served modules used in development.

## Past mistakes & points to improve

- Robin's complaint from this session: terse parenthetical asides lose him. The example was "Production still bundles (via Rollup)", which says almost nothing about what actually happens.
- Standing rule from this session: use full sentences, assume no context, and expand every acronym on first use. When explaining a tool, spell out the mechanism in plain words instead of naming the tool inside parentheses.
