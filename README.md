# pod-pipewire

The `pipewire` candy of the OpenCharly candy library, as a standalone repo (the
candy de-submodule cutover, kind-prefixed naming). It installs the PipeWire
audio/media server with PulseAudio compatibility and the WirePlumber session
manager, supervised in the container.

## What it provides

Installs PipeWire, the `pipewire-pulse` PulseAudio compatibility shim, the `pw-*`
control utilities, and WirePlumber, then copies a `pipewire-wrapper` launch script
into the user's local bin. A user-scope supervisord service runs that wrapper,
which starts `pipewire` + `wireplumber` + `pipewire-pulse`, so container desktops
(the `sway-desktop` composition) get working audio.

| Property | Value |
|---|---|
| Service | `pipewire` (`~/.local/bin/pipewire-wrapper`, `restart: always`, priority 5) |
| Requires | `layer-supervisord` |
| Install files | `pipewire-wrapper` (copied to `~/.local/bin`) |
| Packages | `pipewire`, `pipewire-pulse` / `pipewire-pulseaudio`, `wireplumber` (arch/fedora) |
| Env | `XDG_RUNTIME_DIR=/tmp` |

## How to use it

Typically composed via the desktop stack rather than used directly:

```yaml
my-desktop:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/pod-pipewire:<tag>'
```

```bash
charly box build my-desktop
charly start my-desktop
```

## Layout

- `charly.yml` — the `pipewire:` candy entity (description, `require`, `distro`,
  `service`, `plan`) plus its `skill:` entity.
- `pipewire-wrapper` — the launch script copied into `~/.local/bin`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:pipewire` — the candy properties, the wrapper,
  and the audio service.
- `/charly-selkies:sway-desktop` — the composition that includes pipewire.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
