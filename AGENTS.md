# AGENTS.md — layer-omarchy-boot

Standalone candy repo for the Omarchy boot chain (limine, snapper, plymouth, sddm)
as a **machine-only** layer. The repo is currently a **scaffold**: it carries only
`README.md` and `.github/workflows/tag-on-merge.yml` — **no `charly.yml` and no
`candy/` directory yet**. There is therefore no entity, no `skill:` entity, and no
owning skill projected into the marketplace corpus. The gap is recorded against
`opencharly/opencharly#291` (the batch that authors missing `skill:` entities).

Canonical files:

- `README.md` — user overview only; never agent guidance.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Load these skills first (R0)

- `/charly-distros:omarchy-base` — the family foundation skill: the package
  sources, the pinned mirror snapshot, and the limine alpm hook `NoExtract` rules
  that keep the boot stack inert in a container. Load before editing or
  troubleshooting.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, and service declarations). Load before authoring the first candy.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- There is nothing to build or validate yet: with no `charly.yml`, `charly box
  validate` has no manifest to parse. The org-wide candy gate
  (`opencharly/.github/.github/workflows/candy-validate.yml`) **skips cleanly** on a
  repo with no `charly.yml`.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- When the first candy lands, its `plan:` `check:` steps become the functional
  evidence and this section must name them.

## Modify this repo

- Author the candy at `candy/omarchy-boot/charly.yml` and add the root `charly.yml`
  repo shape (`discover:`) in the same change — a repo without a root project
  manifest is not scanned.
- Keep the layer **machine-only**. limine installs to a real ESP, snapper needs
  btrfs subvolumes, plymouth lives in an initramfs, and sddm claims a seat; none
  belongs in a pod.
- There is no `skill:` entity to keep in sync yet; if one is added (per #291), it
  must be edited together with the candy entity in the same change.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
