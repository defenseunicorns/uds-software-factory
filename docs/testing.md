# SWF Testing

Currently we test the UDS Software Factory at two different levels, the overall `bundle`, and the individual `package`.  Each level has different tests that it performs in order to save time while still ensuring that the necessary configurations are covered.

## Bundle Testing

Bundle testing is performed in the main [UDS Software Factory repository](https://github.com/defenseunicorns/uds-software-factory/) and tests the following:

1. Overall health of each package within the deployment
2. Integration of individual packages with each other
3. Integration of packages with all of `uds-core`
4. Integration of packages with clusters beyond `k3d`

It does not test:

1. Upgrades of individual packages
2. Package flavors other than `upstream`

## Package Testing

Package testing is performed in each package repository and tests the following:

1. Health of the Package deployment (including its individual components)
2. Package upgrades from the previous version to the current version
3. The full matrix of package flavors
4. Integration of a package with `k3d-core-istio`

It does not test:

1. Integration between SWF packages (with the exception of add-ons like GitLab Runner)
2. Integration of a package with all of `uds-core`
3. Integration of packages with clusters beyond `k3d`

> :warning: **Note:** In order to ensure that we can be reasonably confident in the above, package flavors should deviate only by their selected _images_ and not by other configuration like their Helm Chart. This ensures that when we test the images with the given chart / configuration and then test that configuration within a bundle, we can be reasonably certain that each combination of flavors would work within the bundle.

## Running Tests

In order to keep them portable, tests at all levels will be implemented as `uds` tasks that create the necessary packages/bundles, setup the cluster, deploy the packages/bundles and then perform the necessary checks on them.  These will then be wrapped in GitHub actions to run on Pull Requests / Releases.

> :warning: **Note:** You can see the list of available tasks in a given repository by running `uds run --list` at the repository root.
