# Changelog

Every release, newest first. Free tier improvements ship to everyone, and Pro
items are marked. Updates within a major version are free.

## v3.3: one licence, one device

**2026-09-10** · Pro

- **Device-bound activation.** A Pro licence now pairs with exactly one Mac.
  Activation takes two strings from the receipt: the licence key plus an
  activation certificate bound to the machine's device identifier, shown in the
  Pro screen. Still verified entirely offline.
- **Moving to a new Mac** is deactivate in the app, then request a fresh
  activation. One email, no charge.
- Copying an activated licence to another Mac does not work: the device
  identifier will not match.
- Under the hood, the whole realism engine now builds and passes its full test
  suite on Windows as well.

## v3.2: the mistake model

**2026-09-02**

- **Typos follow finger anatomy.** Same-finger column slips dominate, mirror-hand
  slips are rare, and the wrong key is a weighted near miss around the intended
  one: usually touching, sometimes two keys off, rarely a wild fat-finger.
- **Keyboard-layout aware.** Typing and slips follow the active macOS layout,
  QWERTZ, AZERTY, Dvorak, Cyrillic, captured at run start.
- **Post-error slowing.** The two keystrokes after a mistake run measurably
  slower, decaying back to normal.
- **New slip class:** perseverations, where an earlier letter intrudes, plus
  number-row and punctuation neighbours.
- **Missed-Shift capitals** (Pro): capitals sometimes land lowercase, either
  caught instantly, repaired at the word's end, or missed entirely.
- **Doubled words** (Pro): "the the", always caught and drummed away.
- **Free tier set to 500-character runs** with core realism. Pro removes the cap
  and unlocks the realism boosters.
- A ninety-check self-test suite now covers the full pipeline, licensing and
  every realism invariant.

## v3.1: the redesign

**2026-08-22**

- **Redesigned main window:** status header, editor card with drag and drop,
  delivery presets, and a reserved progress strip so nothing reflows mid-run.
- **Menu bar mission control:** live progress, speed, remaining time,
  pause and resume, and a quick speed slider, from the clock corner.
- **Pause and resume mid-run,** with paused time excluded from every statistic.
- **Offscreen render harness:** 58 UI screenshots verified per release.
- **Delivered-speed calibration:** the slider means what it says at every
  setting from 30 to 120 words per minute.

## v3.0: the realism engine

**2026-08**

- **Timing model rebuilt on published research:** skewed inter-key intervals,
  correlated burst inertia, digraph latencies, word tiers, warm-up and drift.
- **Error taxonomy:** substitutions, omissions, transpositions and
  double-strikes, with immediate, delayed and never-noticed corrections.
- **Curved mouse glides** with Fitts-law timing and endpoint scatter.
- **Secure Input auto-pause** on password fields, and the stop-on-app-switch
  guard.
