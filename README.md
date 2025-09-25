
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

git tag runtime-r44_4.4.3-12
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
IMAGE=tercen/dart:3.5.3-1
docker build -t ${IMAGE} dart
docker push ${IMAGE}
```

# runtime r

```shell
IMAGE=tercen/runtime-r40:4.0.4-6
docker build -t ${IMAGE} runtime-r40
docker push ${IMAGE}
```

# runtime r44

```
git tag runtime-r44_4.4.3-7
git push --tags
```

## tensorflow

```shell
IMAGE=tercen/runtime-tf:2.17.0-gpu
docker build -t ${IMAGE} runtime-tf
docker push ${IMAGE}

git tag runtime-tf_2.17.0-gpu
```