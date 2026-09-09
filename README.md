# Factorio MOD Scaffold

This project serves as a scaffold for creating Factorio MODs.

## Prerequisites

- [zsh](https://www.zsh.org/)
- [mise](https://mise.jdx.dev/)
- A configured Git identity (`git config user.name` and `user.email`) — used for the initial commit's authorship and, when `MOD_LICENSE=default_mit`, embedded as the `LICENSE.txt` copyright holder
- [gh CLI](https://cli.github.com/) authenticated (`gh auth login`) with admin access to the new repository — `./bin/initialize` changes repository settings, creates the `release` environment, and sets its secret
- A Factorio Mod Portal API key with the [`ModPortal: Publish Mods`](https://wiki.factorio.com/Mod_publish_API), [`ModPortal: Upload Mods`](https://wiki.factorio.com/Mod_upload_API), and [`ModPortal: Edit Mods`](https://wiki.factorio.com/Mod_details_API) usages, created at https://factorio.com/profile — `./bin/initialize` will prompt for this to set the `FACTORIO_API_KEY` secret
- Optionally, an [Anthropic API key](https://console.anthropic.com/) — `./bin/initialize` will prompt for it to set the `ANTHROPIC_API_KEY` secret that runs the weekly scaffold-drift workflow. Leave it blank to skip; the workflow no-ops until the secret is set.

## Usage

1. Create a new repository from this template

   - On GitHub, click the [Use this template](https://github.com/sakuro/factorio-mod-scaffold/generate) button, then clone the new repository
   - Or, using the [gh CLI](https://cli.github.com/): `gh repo create <new-repo-name> --template sakuro/factorio-mod-scaffold --clone`

2. `cd` into the new repository directory

3. Run `mise trust`

4. Run `./bin/initialize`

   The script performs the following:

   1. Install tools via `mise install`
   2. Generate MOD files (`info.json`, `locale/en/<MOD_NAME>.cfg`, `changelog.txt`)
   3. Replace `README.md` with the MOD title and template repository link
   4. Download and set up `LICENSE.txt` based on the specified license
   5. Write release settings (`MOD_LICENSE`, `MOD_CATEGORY`, `MOD_TAGS`) into `mise.toml`
   6. Configure GitHub repository settings

      - Auto-delete merged branches
      - Auto-merge for pull requests
      - GitHub Discussions
      - Repository topics (`factorio-mod`)
      - Repository website (MOD portal page)
      - Issue labels (project-specific additions; see [Issue labels](#issue-labels))
      - Branch protection requiring pull requests
      - Workflow permissions
      - `release` environment
      - `FACTORIO_API_KEY` secret
      - `.scaffold-sync.json` baseline and the `ANTHROPIC_API_KEY` secret for the scaffold-drift workflow
   7. Remove `bin/initialize` itself and amend the initial commit

   - `MOD_LICENSE` can be set to customize the license (defaults to `default_mit`); see [License identifier](https://wiki.factorio.com/Mod_details_API#License)
   - Example: `MOD_LICENSE=default_gnulgplv3 ./bin/initialize`
   - MOD name, title, and author are derived from the current directory name and the GitHub repository owner; edit `info.json` and `locale/en/<MOD_NAME>.cfg` afterward if they need to be different. MOD category and tags are written to `mise.toml` with their default values (`no-category` and empty, respectively) and can be edited there.

5. Your MOD scaffold will be ready

## Issue labels

GitHub provisions its standard label set (`bug`, `documentation`, `duplicate`,
`enhancement`, `good first issue`, `help wanted`, `invalid`, `question`,
`wontfix`, `accessibility`) on every new repository. `./bin/initialize` adds
four project-specific labels:

| Label | Color | Description |
| --- | --- | --- |
| `chore` | `e4b429` | Maintenance, tooling, and housekeeping |
| `test` | `0e8a16` | Tests and test infrastructure |
| `refactor` | `d4c5f9` | Code restructuring without changing external behavior |
| `icebox` | `add8e6` | Parked for the future; not scheduled for work |
| `run-ci` | `1d76db` | Add to a scaffold-drift PR to run CI |

An initialized repository ends up with 15 labels: the 10 standard ones plus these five. `run-ci` is a PR-control label rather than an issue label.

## Scaffold drift

`.github/workflows/scaffold-drift.yml` runs weekly in each generated MOD and, via
[Claude Code Action](https://github.com/anthropics/claude-code-action), opens a
`chore/scaffold-drift` PR when this scaffold's shared infrastructure has moved
ahead of the MOD. It is gated on the `ANTHROPIC_API_KEY` secret and does nothing
on a fork. See `CONTRIBUTING.md` for how to review those PRs.
