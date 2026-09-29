# AGENTS.md — pod-pipewire

Standalone candy repo for the `pipewire` candy — the PipeWire audio/media server
with PulseAudio compatibility and the WirePlumber session manager, supervised in
the container. The candy lives in `charly.yml` at the repo root plus its launch
wrapper.

Canonical files:

- `charly.yml` — the `pipewire:` candy entity (description, `require`, `distro`,
  `service`, `plan`) and its `skill:` entity.
- `pipewire-wrapper` — the launch script copied into `~/.local/bin`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:pipewire` — the owning skill: the candy properties, the
  wrapper, the audio service, and verification. Load before editing, building,
  deploying, or troubleshooting this candy.
- `/charly-selkies:sway-desktop` — the composition that includes pipewire.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `copy:` / `check:`, service declarations).
- `/charly-check:check` — the check/R10 framework (`charly check box`,
  `charly check run <bed>`).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own `check:` steps assert the `pipewire`, `wireplumber`,
  `pipewire-pulse`, and `pw-cli` binaries, the installed wrapper, and the running
  `pipewire` service.

## Modify this repo

- Edit the `pipewire:` candy entity in `charly.yml`; the `skill:` entity in the
  same file is the owning skill's source — a candy change and its skill change
  land together.
- The `pipewire-wrapper` script is copied by the `copy:` step; keep its
  destination and mode in step with the service `exec` path.
- Keep the service `scope: user` — PipeWire and WirePlumber need the user session
  bus and per-user runtime dir.
- The `skill:` entity is the source for `/charly-selkies:pipewire`; never edit the
  generated `SKILL.md` — regenerate it.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
