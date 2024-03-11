# SWF Versioning

> :warning: **Note:** Packages are the only production release artifacts published from the UDS Software Factory repositories, as bundles are meant to be _created_, not consumed, by users.  Repos may publish bundles for demo purposes, but those are not intended for production consumption though will loosely follow standard semantic versioning.

## Versioning

UDS Software Factory `packages` are versioned as follows:

```
<upstream-app-version>-uds.<uds-sub-version>
```

Where,

- `upstream-app-version`: is the version of the main application in the package (i.e. `16.9.2` for GitLab)
- `uds-sub-version`: is the number of releases since the last main application version bump (starting at `0`)

In practice, this results in the following for the second release of a `package` for version `16.9.2` of GitLab:

```
16.9.2-uds.1
```

## Releases

Releases themselves are cut with [Release Please](https://github.com/googleapis/release-please), but due to the fact that we don't follow semantic versioning precisely, specific version numbers are manually bumped in the release please PRs or via [empty commits](https://github.com/googleapis/release-please).  Specific lines to update within `zarf.yaml` or `uds-bundle.yaml` files are controlled via the following:

```yaml
  # x-release-please-start-version
  version: "0.2.1"
  # x-release-please-end
```

Once a release is cut by merging a release-please PR, a GitHub action takes over and publishes the resulting package to `ghcr.io/defenseunicorns/packages` where it is available in Zarf and UDS CLI via the `defenseunicorns` and `🦄` shorthands.

> :warning: **Note:** If a release is broken or has known issues that the team feels are severe enough, the `CHANGELOG.md` and GitHub release notes should be updated denoting those facts, and a new release should be issued as soon as the team is able.
