# Verified on real devices

## Status

active since 2026-Q4

## Outcome

What the family claims for a platform holds on real devices there, not
only in Dart logic and loopbacks on the developer's Mac. App developers
can rely on the
[composition per platform](../2026-Q4/architecture/architecture.md#composition-per-platform)
without re-testing each platform themselves.

## Measure

- The verification table of the architecture has no cell `not verified`
  for a shipped transport
- Linux and Windows run their native paths on real machines
- USB and Bluetooth LE are verified with hardware on at least one mobile
  and one desktop platform

## Requirements

- [req-constraints-004: Checks green on three operating systems](../requirements/constraints.md#req-constraints-004-checks-green-on-three-operating-systems)
- [req-engine-007: One contract for every backend](../requirements/engine.md#req-engine-007-one-contract-for-every-backend)
- [req-platforms-001: iOS and macOS](../requirements/platforms.md#req-platforms-001-ios-and-macos)
- [req-platforms-002: Android](../requirements/platforms.md#req-platforms-002-android)
- [req-platforms-003: Windows](../requirements/platforms.md#req-platforms-003-windows)
- [req-platforms-004: Linux](../requirements/platforms.md#req-platforms-004-linux)
- [req-platforms-005: Web](../requirements/platforms.md#req-platforms-005-web)
