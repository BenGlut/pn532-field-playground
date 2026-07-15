# PN532 Field Placement Simulator

A self-contained, single-file 3D playground for reasoning about PN532 (13.56 MHz NFC/RFID)
antenna placement — how far a tag can sit, how much tilt/offset it tolerates, how a housing
wall and nearby electronics affect range, and what happens when two readers face each other.

Open [`index.html`](index.html) directly in a browser (or via GitHub Pages) — no build step,
no dependencies, everything (physics, 3D renderer, UI) is in one HTML file.

## What it simulates

- **Square-trace antenna physics** — the reader coil is modelled as the actual rounded-square
  PCB trace a PN532 module uses (not a circular approximation), with the field computed by
  numerically integrating the Biot–Savart law around that path at any 3D point.
- **3D field lines & donut envelope** — the coil's real closed field lines, plus an iso-surface
  ("donut") tracing the boundary where coupling equals a chosen read threshold. Colour-coded
  front (toward the tag) vs. back (into the housing/electronics).
- **PN532 board sprite** — a to-scale visual reference of the board itself (chip, mounting
  holes, pin header) rendered in the same 3D scene, depth-composited with the field envelope.
- **Drag-to-position tag** — drag the NTAG215 tag directly in the 3D view to move it sideways;
  hold Shift while dragging to push it closer/farther. A live badge next to the tag shows the
  read-quality estimate as you move it.
- **Reader power** — a gain slider that scales the coil's drive current, growing/shrinking the
  donut and coupling percentage without changing the field lines' shape.
- **Dual-reader playground** — turn on a second PN532 facing the first and set the spacing
  between them to see how their fields add/cancel (a proxy for cross-talk), and check whether
  a tag placed between them still reads at a given separation.

## What it is not

The coupling/exposure numbers are **relative**, not calibrated field strengths — real range
depends on your board's matching network, driver gain, and nearby metal/ferrite. The
dual-reader mode sums the two coils' fields by superposition; it does not simulate mutual
inductance or impedance detuning between two real, physically coupled antennas. Use this to
reason about geometry, then confirm with real hardware.

## Tech

Plain HTML/CSS/JS, no build tooling, no external libraries — the 3D rendering (projection,
depth-sorted painter's algorithm, field-line tracing) is hand-rolled on a single `<canvas>`.
