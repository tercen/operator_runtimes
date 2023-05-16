
Docker containers runtime environment for Tercen operators.

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

You can build and push runtime images automatically using GitHub Actions using tags:

```
git tag runtime-r35_3.5.3-7
git push --tags

git tag runtime-r40_4.0.4-6
git push --tags

git tag runtime-r40-slim_4.0.4-1
git push --tags

git tag runtime-flowsuite_3.15-3
git push --tags

git tag tercen/runtime-matlab-image_r2020b-2
git push --tags
```

# dind

```bash
IMAGE=tercen/dind
docker build -t ${IMAGE} dind
docker push ${IMAGE}
```

# dart

```shell
IMAGE=tercen/dart:2.19.6-1
docker build -t ${IMAGE} dart
docker push ${IMAGE}
```

# runtime

```shell
IMAGE=tercen/runtime-r40:4.0.4-5
docker build -t ${IMAGE} runtime-r40
docker push ${IMAGE}
```