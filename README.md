# Conventional Changelog action

This action will bump version, tag commit and generate a changelog with conventional commits.

## Inputs

- **Required** `github-token`: Github token.
- **Optional** `git-message`: Commit message that is used when committing the changelog.
- **Optional** `git-user-name`: The git user.name to use for the commit. Default `Conventional Changelog Action`
- **Optional** `git-user-email`: The git user.email to use for the commit. Default `conventional.changelog.action@github.com`
- **Optional** `git-pull-method`: The git pull method used when pulling all changes from remote. Default `--ff-only`
- **Optional** `preset`: Preset that is used from conventional commits. Default `angular`.
- **Optional** `tag-prefix`: Prefix for the git tags. Default `v`.
- **Optional** `output-file`: File to output the changelog to. Default `CHANGELOG.md`, when providing `'false'` no file will be generated / updated.
- **Optional** `release-count`: Number of releases to preserve in changelog. Default `5`, use `0` to regenerate all.
- **Optional** `version-file`: The path to the file that contains the version to bump. Default `./package.json`.
- **Optional** `version-path`: The place inside the version file to bump. Default `version`.
- **Optional** `skip-on-empty`: Boolean to specify if you want to skip empty release (no-changelog generated). This case occured when you push `chore` commit with `angular` for example. Default `'false'`.
- **Optional** `skip-version-file`: Do not update the version file. Default `'false'`.
- **Optional** `skip-commit`: Do create a release commit. Default `'false'`.

## Outputs

- `changelog`: The generated changelog for the new version.
- `clean_changelog`: The generated changelog for the new version without the version name in it (Better for Github releases)
- `version`: The new version.
- `tag`: The name of the generated tag.
- `skipped`: Boolean (`'true'` or `'false'`) specifying if this step have been skipped

## Example usages

Uses all the defaults

```yaml
- name: Conventional Changelog Action
  uses: TriPSs/conventional-changelog-action@v3
  with:
    github-token: ${{ secrets.github_token }}
```

Overwrite everything

```yaml
- name: Conventional Changelog Action
  uses: TriPSs/conventional-changelog-action@v3
  with:
    github-token: ${{ secrets.github_token }}
    git-message: 'chore(release): {version}'
    git-user-name: 'Awesome Changelog Action'
    git-user-email: 'awesome_changelog@github.actions.com'
    preset: 'angular'
    tag-prefix: 'v'
    output-file: 'MY_CUSTOM_CHANGELOG.md'
    release-count: '10'
    version-file: './my_custom_version_file.json' // or .yml, .yaml, .toml
    version-path: 'path.to.version'
    skip-on-empty: 'false'
    skip-version-file: 'false'
    skip-commit: 'false'
```

No file changelog

```yaml
- name: Conventional Changelog Action
  uses: TriPSs/conventional-changelog-action@v3
  with:
    github-token: ${{ secrets.github_token }}
    output-file: 'false'
```

Tag only

```yaml
- name: Conventional Changelog Action
  uses: TriPSs/conventional-changelog-action@v3
  with:
    github-token: ${{ secrets.github_token }}
    skip-commit: 'true'
```

Github releases

```yaml
- name: Conventional Changelog Action
  id: changelog
  uses: TriPSs/conventional-changelog-action@v3
  with:
    github-token: ${{ secrets.github_token }}
    output-file: 'false'
    skip-on-empty: 'true'

- name: Create Release
  uses: actions/create-release@v1
  if: ${{ steps.changelog.outputs.skipped == 'false' }}
  env:
   GITHUB_TOKEN: ${{ secrets.github_token }}
  with:
   tag_name: ${{ steps.changelog.outputs.tag }}
   release_name: ${{ steps.changelog.outputs.tag }}
   body: ${{ steps.changelog.outputs.clean_changelog }}
```

## Development
If you'd like to contribute to this project, all you need to do is clone and install [act](https://github.com/nektos/act) this project and run:

```shell
$ yarn install

# We need the full 18 gb image because we use GIT
$ act -P ubuntu-latest=nektos/act-environments-ubuntu:18.04 -s github_token=<your token>
```

## [License](./LICENSE)

Conventional Changelog Action is [MIT licensed](./LICENSE).

## Collaboration

If you have questions or [issues](https://github.com/TriPSs/conventional-changelog-action/issues), please [open an issue](https://github.com/TriPSs/conventional-changelog-action/issues/new)!
