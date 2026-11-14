# Repaint / Reflow

**Recorded:** 2026-09-29

## Context

Concept session on repaint and reflow, September 29, 2026.

## Repaint and reflow

A reflow is when the browser recalculates the layout of elements on the page, meaning where each element sits and how big it is. A repaint is when the browser redraws the pixels of an element without changing its layout. A reflow always triggers a repaint, because the layout changed and the pixels must be redrawn. A repaint never triggers a reflow, because only the appearance changed and the layout stayed the same.

## Layout thrashing

Layout thrashing happens when code reads a layout value and then writes a style, over and over in a loop. Each read forces the browser to run a reflow so the read value is up to date, and each write makes the next read stale again. The fix is to batch all reads together first, then batch all writes together, so the browser reflows once instead of once per iteration.

## What to animate

Animate the `transform` and `opacity` properties, not `top`, `left`, or `width`. Changing `transform` or `opacity` can be handled by the compositor, which is the part of the browser that moves already-drawn layers around, without a reflow or a repaint of the page. Changing `top`, `left`, or `width` forces a reflow every frame.

## Past mistakes & points to improve

- None outstanding. This was a clean concept session with no mistakes to record.
