# AGENTS.md — layer-a11y-tools

Standalone candy repo for the `a11y-tools` layer. The whole candy lives in
`charly.yml` at the repo root: its `require:` and package sections, its ordered
`plan:` of build-time `check:` steps, and the embedded `skill:` entity projected
into the marketplace corpus as `/charly-selkies:a11y-tools`. There is no source
tree and no runtime service.

Canonical files:

- `charly.yml` — the `a11y-tools:` candy entity and the `a11y-tools-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:a11y-tools` — the owning skill: what the layer installs, why
  the system interpreter is required, and how `wl: atspi` consumes it. Load
  before editing or troubleshooting the candy.
- `/charly-check:wl` — the `wl: atspi` check verb (`tree`/`find`/`click`) this
  layer backs. Load when changing what the layer exposes to checks.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
  Keep the `version:` schema stamp within the installed charly's supported range.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has no
  per-repo candy gate.
- There is no live bed: the candy is package-only, so the evidence is its
  `plan:` `check:` steps — that `/usr/bin/python3` imports `pyatspi` and `gi`
  and that each distro's package is installed.

## Modify this repo

- Edit the `a11y-tools:` candy entity AND the `a11y-tools-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a package
  or behaviour change that is not mirrored in the skill leaves the corpus stale.
- Package changes go under `distro:`; behaviour claims go in `plan:` as
  observable `check:` steps using the absolute `/usr/bin/python3`.
- Keep `version:` at the schema stamp the pinned CI charly supports.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
