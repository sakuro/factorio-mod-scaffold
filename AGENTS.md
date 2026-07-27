# Project

Factorio MOD.

# Development

## Build and Install

- `mise run install` - Install to local Factorio MOD directory. Uses `git archive` internally, so only committed files are included — commit changes before running.

## Release

Releases are handled by GitHub Actions workflows. Do not run `mise run release:*` tasks manually.

Changelog is managed by `factorix mod changelog` and follows Factorio's changelog.txt specification.

### What to write in changelog.txt

- Regular releases: limit entries to user-visible changes only.
- Initial release: write "Initial release" only, under the `Features` category.

### Updating the changelog during development

Write entries in the Unreleased section at the top of the file. If no Unreleased section exists, create one.

Do not create a section for the next release version directly — version bumping is handled by the GitHub Actions release workflow.

### Pre-release cleanup

Before the first release, remove the scaffold's commented-out placeholder code from `settings.lua`, `data.lua`, and `control.lua`. If a file ends up empty, delete it.

# Document Map

- README.md: Project overview
- CONTRIBUTING.md: Pull request guidelines

# External References

- [Factorio API](https://lua-api.factorio.com/latest/)
- [Factorio Wiki](https://wiki.factorio.com/)
- [factorio-data](https://github.com/wube/factorio-data) — base game's data definitions; clone locally if needed
