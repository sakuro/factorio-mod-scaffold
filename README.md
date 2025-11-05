# Factorio MOD Scaffold

This project serves as a scaffold for creating Factorio MODs.

## Prerequisites

- [zsh](https://www.zsh.org/)
- [aqua](https://aquaproj.github.io/) - Installs the following tools via `aqua.yaml`:
  - [gh](https://cli.github.com/) - GitHub CLI
  - [mise](https://mise.jdx.dev/) - Runtime version manager (installs Ruby via `mise.toml`)
  - [actionlint](https://github.com/rhysd/actionlint) - GitHub Actions workflow linter
  - [shellcheck](https://www.shellcheck.net/) - Shell script linter

## Usage

1. Clone this repository
2. Run `./bin/initialize`
   - The following environment variables can be set to customize initialization:

     | Variable | Default | Description |
     |---|---|---|
     | `MOD_NAME` | Current directory name | MOD's internal name |
     | `MOD_TITLE` | Title-cased `MOD_NAME` | MOD's display title |
     | `AUTHOR_NAME` | GitHub repository owner | Author name for info.json |
     | `MOD_LICENSE` | `default_mit` | [License identifier](https://wiki.factorio.com/Mod_details_API#License) |
     | `MOD_CATEGORY` | *(empty)* | [MOD category](https://wiki.factorio.com/Mod_details_API#Category) |
     | `MOD_TAGS` | *(empty)* | [MOD tags](https://wiki.factorio.com/Mod_details_API#Tags) (comma-separated) |

   - Example: `MOD_LICENSE=default_gnulgplv3 ./bin/initialize`

   The script performs the following:

   1. Install tools and dependencies (`aqua install`, `bundle install`)
   2. Generate MOD files (`info.json`, `locale/en/<MOD_NAME>.cfg`)
   3. Replace `README.md` with the MOD title and template repository link
   4. Download and set up `LICENSE.txt` based on the specified license
   5. Expand ERB templates in `Rakefile` with release settings
   6. Remove `bin/initialize` itself and amend the initial commit

3. Your MOD scaffold will be ready
