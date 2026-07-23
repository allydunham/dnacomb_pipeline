# DNAComb release image

This folder contains the build system to generate Docker/Singularity images for DNAComb and pipeline releases.

For a normal release run:

```bash
make release
```

`make release` checks crates.io, skips existing DockerHub tags, builds missing `dnacomb` versions on `mercury/dnacomb:base`, and pushes them to `<dockerhub-user>/dnacomb:<version>`.

Prepare local tools and DockerHub login only:

```bash
make init
```

The base is fixed and reused, including the other tools needed for the pipeline:

```bash
mercury/dnacomb:base
```

Do not rebuild the base for normal releases. Rebuild it only if `mercury/dnacomb:base` is lost:

```bash
BUILD_BASE=1 BASE_IMAGE=mercury/dnacomb:base ./build-dnacomb-image.sh
```

Build one local `dnacomb` version on the fixed base:

```bash
DNACOMB_VERSION=1.0.0 IMAGE_TAG=dnacomb:1.0.0 ./build-dnacomb-image.sh
```

Use another destination with:

```bash
IMAGE_REPO=my-org/dnacomb make release
```

Use released images from the pipeline:

```bash
DNACOMB_VERSION=1.0.0 nextflow run . -with-singularity -config examples/example.config
```
