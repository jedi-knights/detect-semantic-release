# detect-semantic-release

[![Coverage](https://img.shields.io/badge/Coverage-0%25-red)](https://jedi-knights.github.io/detect-semantic-release/)

A GitHub Action that wraps [`jedi-knights/go-semantic-release`](https://github.com/jedi-knights/go-semantic-release) and exposes `released`, `version`, and `tag` outputs — which the underlying action does not provide.

## Why

`go-semantic-release` runs semantic release but gives you no way to know whether a release actually happened. Every caller ends up writing the same VERSION-comparison workaround. This action does that once so you don't have to.

## Usage

```yaml
- uses: actions/checkout@v6
  with:
    fetch-depth: 0
    token: ${{ secrets.GH_TOKEN }}

- name: Configure git
  run: |
    git config user.name "semantic-release-bot"
    git config user.email "semantic-release-bot@users.noreply.github.com"

- uses: jedi-knights/detect-semantic-release@v0
  id: release
  with:
    github_token: ${{ secrets.GH_TOKEN }}

- name: Build and publish
  if: steps.release.outputs.released == 'true'
  run: echo "Released ${{ steps.release.outputs.tag }}"
```

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `github_token` | yes | — | Token for semantic-release API calls and git operations |
| `version_file` | no | `VERSION` | File compared before/after to detect a new release |
| `args` | no | `''` | Additional arguments passed to go-semantic-release (e.g. `--dry-run`) |

## Outputs

| Output | Description |
|---|---|
| `released` | `"true"` if a new version was released, `"false"` otherwise |
| `version` | New version number (e.g. `"1.2.3"`). Empty string if no release. |
| `tag` | New git tag (e.g. `"v1.2.3"`). Empty string if no release. |
