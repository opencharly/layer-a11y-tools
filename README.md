# a11y-tools

AT-SPI2 accessibility introspection for OpenCharly desktop images.

The `a11y-tools` candy installs the Python AT-SPI2 bindings (`pyatspi`) and
PyGObject (`gi`) into the **system** Python 3, so a candy or a check can query
the accessibility tree of GTK, Qt, and Chrome applications — finding elements by
name and role instead of pixel coordinates. It backs the `wl: atspi` check verb
(`tree` / `find` / `click`).

The layer is package-only: no service, no daemon, no runtime port. It requires
the D-Bus session bus (`pod-dbus`), without which AT-SPI2 cannot reach the
desktop's accessibility bus.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `a11y-tools` |
| Requires | `@github.com/opencharly/pod-dbus` (the D-Bus session bus) |
| Packages | Fedora: `python3-pyatspi`, `python3-gobject` · Arch: `python-atspi`, `python-gobject` |
| Python | the system `/usr/bin/python3` (NOT the pixi env that dominates PATH) |
| Service / port | none |
| Environment | none |

## How to use it

Compose the layer in a box's `candy:` list:

```yaml
my-desktop:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-a11y-tools:<tag>'
```

Then, inside the built image, import the bindings from the system interpreter:

```bash
/usr/bin/python3 -c "import pyatspi, gi; print('a11y ready')"
```

Always use the absolute `/usr/bin/python3`: containers with a pixi environment
have pixi's Python first on PATH, and it does not see the system packages.

## Layout

- `charly.yml` — the `a11y-tools:` candy entity (`require:`, package sections,
  `plan:` checks) and the embedded `a11y-tools-skill:` skill entity.
- `.github/workflows/` — the org-wide `charly/pr-validator` gate; no per-repo candy gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:a11y-tools`
- Check verb: `/charly-check:wl` (`wl: atspi`)
- D-Bus: `/charly-infrastructure:dbus-layer`
- Bundled by: `/charly-selkies:selkies-desktop-layer`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
