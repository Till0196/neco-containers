[![CircleCI](https://circleci.com/gh/cybozu/neco-containers.svg?style=svg)](https://circleci.com/gh/cybozu/neco-containers)

# Neco Containers

This repository contains Dockerfiles to build OSS products
used in our project, Neco.  They are built from the official
sources, and based on our Ubuntu base image.

See also: [github.com/cybozu/ubuntu-base](https://github.com/cybozu/ubuntu-base).

Built images can be pulled from [quay.io/cybozu][quay].

## How it works

Subdirectories in this repository have `TAG` and `BRANCH` files
in addition to files to build Docker images.

These will be used by CircleCI to tag the built images.
CircleCI does the following each time commits are pushed to a branch.

1. For each directory containing `TAG` file:
    1. Read `TAG` file and check if the repository at [quay.io/cybozu][quay] with the same name of the directory.
    2. If the repository contains the same tag in `TAG`, continue to the next directory.
    3. Otherwise, build a Docker image using `Dockerfile` under the directory.
2. If the branch is not `main`, CircleCI stops here without pushing.
3. If the branch is `main`, for each directory with a built image:
    1. Tag the built image with tag in `TAG` file.
    2. Push the tagged image to quay.io.
    3. If `TAG` represents a pre-release such as `1.2-rc.1`, continue to the  next directory.
    4. If the directory contains `BRANCH` file:
        1. Tag the built image with tag in `BRANCH` file.
        2. Push the tagged image to quay.io.

## Tag naming

Images whose upstream version conform to [Semantic Versioning 2.0.0][semver] should be
tagged like this:

    Upstream version + "." + Container image version

For example, if the upstream version is `X.Y.Z`, the first image for this version will
be tagged as `X.Y.Z.1`.  Likewise, if the upstream version has pre-release part like
`X.Y.Z-beta.3`, the tag will be `X.Y.Z-beta.3.1`.
The container image version will be incremented when some changes are introduced to the image.

If the upstream version has no patch version (`X.Y`), fill the patch version with 0 then
add the container image version _A_ (`X.Y.0.A`).

If the upstream is a Debian package, the format of upstream version is `X.Y.Z-PACKAGE`
where `PACKAGE` is the debian package version.  In this case, use `X.Y.Z.PACKAGE` as
the package version and add the container image version _A_ (`X.Y.Z.PACKAGE.A`).

The container image version _must_ be reset to 1 when the upstream version is changed.

### Example

If the upstream version is "1.2.0-beta.3", the image tag must begin with "1.2.0-beta.3.1".

## Branch naming

If the image is built for an upstream version X.Y.Z, the branch name _should_ be X.Y
for X > 0, or "0" for X == 0.

[quay]: https://quay.io/organization/cybozu
[semver]: https://semver.org/
