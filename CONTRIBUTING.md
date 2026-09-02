# Contributing

When opening a pull request:

- Do not change the version in `info.json`. Version bumping is handled by the release workflow.
- Document any user-visible change in `changelog.txt` (see below).

## Changelog

`changelog.txt` follows Factorio's changelog format. Between releases it has no
`Unreleased` section — its top section is the last released version.

For the first user-visible change of a new cycle, prepend an `Unreleased` section:

```
---------------------------------------------------------------------------------------------------
Version: Unreleased
  Changes:
    - Describe the change here.
```

Add later entries to that same section. Do not create a section for the next
version number — the release workflow does the version bump.
