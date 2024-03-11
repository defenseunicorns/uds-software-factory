# SWF Versioning

> :warning: **Note:** Packages are the only artifacts published from the UDS Software Factory repositories, as bundles are meant to be _created_, not consumed, by users.

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

Releases themselves are cut with [Release Please](https://github.com/googleapis/release-please) but due to the fact that we don't follow semantic versioning precisely, specific version numbers are manually bumped in PRs or via [empty commits](https://github.com/googleapis/release-please).

Once a release is cut, a GitHub action takes over and publishes the resulting package to `ghcr.io/defenseunicorns/packages` where it is available in Zarf and UDS CLI via the `defenseunicorns` and `🦄` shorthands.
