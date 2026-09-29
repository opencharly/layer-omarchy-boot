# layer-omarchy-boot

The Omarchy boot chain as a charly layer — **limine, snapper, plymouth and sddm** —
**machine-only**, not yet populated.

## Status

This repo is a **scaffold**: it carries this `README.md` and the org-wide
`.github/workflows/tag-on-merge.yml` dispatcher, and **no `charly.yml` or candy yet**.
There is no `candy/omarchy-boot/` to pin, and no image composes it. The scope below
is the intended role, recorded so the first candy lands against it.

## Intended scope

The Omarchy base image installs the whole boot stack *inertly*: the `omarchy`
package hard-depends on `limine`, `limine-mkinitcpio-hook`, `limine-snapper-sync`,
`snapper` and `sddm`, so a container gets them regardless — but the alpm hooks that
would drive `limine-entry-tool` are never extracted (`omarchy-base` adds five
`NoExtract` rules) and no unit is enabled. A container cannot have a genuinely
mounted FAT32 ESP, so the boot chain is left inert by design.

This layer is where the **machine** surfaces belong: a real limine install on a
real ESP, snapper subvolumes, plymouth in an initramfs, and an sddm seat. It is
machine-only and opt-in — no pod composes it.

## How it is meant to be consumed

Once populated, a machine image composes the candy by pinning its sub-path in the
nested `candy:` list:

```yaml
my-omarchy-machine:
  candy:
    base: omarchy
    candy:
      - '@github.com/opencharly/layer-omarchy-base/candy/omarchy-base:v2026.242.0701'
      - '@github.com/opencharly/layer-omarchy-boot/candy/omarchy-boot:<tag>'
```

## Layout

- `README.md` — this user overview.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: none yet — `/charly-distros:omarchy-base` is the closest family
  owning procedure (the limine `NoExtract` rules). The missing `skill:` entity is
  recorded against [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-distros:omarchy` — the Omarchy base image that installs the stack inertly.
- `/charly-distros:omarchy-base` — the foundation layer and its limine-hook rules.
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella.
