# Factorio MOD Scaffold

This project serves as a scaffold for creating Factorio MODs.

## Prerequisites

- [zsh](https://www.zsh.org/)
- [mise](https://mise.jdx.dev/)
- A Factorio Mod Portal API key with the [`ModPortal: Publish Mods`](https://wiki.factorio.com/Mod_publish_API), [`ModPortal: Upload Mods`](https://wiki.factorio.com/Mod_upload_API), and [`ModPortal: Edit Mods`](https://wiki.factorio.com/Mod_details_API) usages, created at https://factorio.com/profile — `./bin/initialize` will prompt for this to set the `FACTORIO_API_KEY` secret

## Usage

1. Create a new repository from this template

   - On GitHub, click the [Use this template](https://github.com/sakuro/factorio-mod-scaffold/generate) button, then clone the new repository
   - Or, using the [gh CLI](https://cli.github.com/): `gh repo create <new-repo-name> --template sakuro/factorio-mod-scaffold --clone`

2. `cd` into the new repository directory

3. Run `mise trust`

4. Run `./bin/initialize`

   The script performs the following:

   1. Install tools via `mise install`
   2. Generate MOD files (`info.json`, `locale/en/<MOD_NAME>.cfg`)
   3. Replace `README.md` with the MOD title and template repository link
   4. Download and set up `LICENSE.txt` based on the specified license
   5. Write release settings (`MOD_LICENSE`, `MOD_CATEGORY`, `MOD_TAGS`) into `mise.toml`
   6. Configure GitHub repository settings (auto-delete merged branches, workflow permissions, `release` environment, `FACTORIO_API_KEY` secret)
   7. Remove `bin/initialize` itself and amend the initial commit

   - `MOD_LICENSE` can be set to customize the license (defaults to `default_mit`); see [License identifier](https://wiki.factorio.com/Mod_details_API#License)
   - Example: `MOD_LICENSE=default_gnulgplv3 ./bin/initialize`
   - MOD name, title, and author are derived from the current directory name and the GitHub repository owner; edit `info.json` and `locale/en/<MOD_NAME>.cfg` afterward if they need to be different. MOD category and tags are written to `mise.toml` with their default values (`no-category` and empty, respectively) and can be edited there.

5. Your MOD scaffold will be ready
