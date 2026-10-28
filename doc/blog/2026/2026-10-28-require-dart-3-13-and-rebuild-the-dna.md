<!--
@license
Copyright (c) audanika

Use of this source code is governed by terms that can be
found in the LICENSE file in the root of this package.
-->

# Require Dart 3.13 and rebuild the DNA

## Motivation

- The MIDI packages are maintained in one pass: every repo moves to the
  current Dart SDK, takes the current versions of its dependencies and
  rebuilds its DNA instances.
- This repo holds the plans, decisions and posts of the family, no code.
  Its share of the pass is the SDK floor its dev dependencies resolve
  against and the DNA instances that drive the planning workflow.
- The instances matter here more than elsewhere: `CLAUDE.md` and the
  guides are what an AI reads before it plans a ticket in this repo, so
  they have to come out of the engine the repo tests them with.

## Strategy

- Raise the SDK constraint to `^3.13.0`, in step with the packages the
  plans describe.
- Let `gg do upgrade deps` confirm that `dna_audanika`, `dna_plan` and
  `helix` are current; they were, so the lock file stays.
- Rebuild the DNA with `gg dna build` and let the DNA test of the repo
  verify the result when the checks run.

## Implementation 1: SDK floor

- `pubspec.yaml`: `environment.sdk` moves from `^3.12.2` to `^3.13.0`.
- Nothing else in the manifest changes; the repo publishes to none.

## Implementation 2: DNA instances

- `gg dna build` of gg 17.6 rebuilt the instances with the helix 1.13
  engine that gg ships: it folded `.claude/CLAUDE_dna_gg.md` and
  `.claude/CLAUDE_dna_plan.md` into `CLAUDE.md` and committed that as
  `#gg: generated DNA`.
- The DNA test of the repo runs with `helix` 1.14, the dev dependency,
  and rebuilt the instances again on the next check: the two files came
  back, `CLAUDE.md` references them as before and the manifest records
  1.14 as the engine. That landed as a second `#gg: generated DNA`
  commit.
- The two commits cancel out: the guides, skills and `CLAUDE.md` of the
  repo are the ones it had before, verified by its own engine.

## Dependency updates

- No dependency changed its version. `dna_audanika` 0.1.0, `dna_plan`
  1.0 and `helix` 1.14 were already current.

## Open Points

- gg 17.6 builds the DNA with helix 1.13 while the repo tests it with
  helix 1.14. Until gg moves to 1.14, `gg dna build` and the DNA test
  disagree about `CLAUDE.md`; the test has the last word.
- The DNA variables of this repo name `audanika` as the git organisation,
  while the repo lives under `audmidi`. The badge URLs of the README
  template point there. A follow-up pass over the MIDI family should set
  `dnaGitOrg` and `dnaGitOrgUrl` to `audmidi`.
