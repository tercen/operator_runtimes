
Docker containers runtime environment for tercen operators.

# Runtimes specifications

## runtime-r35
- based on rocker/tidyverse:3.5.3
- rust 1.40.0
- dart 2.10.5-1
- python 3.6

## runtime-r40
- based on rocker/tidyverse:4.0.3
- rust 1.40.0
- dart 2.10.5-1
- python 3.8
- CRAN=https://cran.tercen.com/api/v1/rlib/CRAN
 
# Build runtimes

You can build and push runtime images automatically using GitHub Actions by editing the `build_runtimes.yml` workflow. In particular, incrementing version number in the images list of the config file will trigger build:

```yml
          - "tercen/runtime-r35:3.5.3-7"
          - "tercen/runtime-r40:4.0.4-4"
          - "tercen/runtime-r40-slim:4.0.4-1"
          - "tercen/runtime-flowsuite:3.15-3"
          - "tercen/runtime-matlab-image:r2020b-2"
```

# dind

```bash
IMAGE=tercen/dind
docker build -t ${IMAGE} dind
docker push ${IMAGE}
```

# dart

```shell
IMAGE=tercen/dart:2.14.4
docker build -t ${IMAGE} dart
docker push ${IMAGE}
```