
Docker containers runtime environment for tercen operators.

# runtime-r35
- based on rocker/tidyverse:3.5.3
- rust 1.40.0
- dart 2.10.5-1
- python 3.6

# runtime-r40
- based on rocker/tidyverse:4.0.3
- rust 1.40.0
- dart 2.10.5-1
- python 3.8
 
# build

```bash
IMAGE=tercen/dind
docker build -t ${IMAGE} dind
docker push ${IMAGE}

IMAGE=tercen/runtime-r35:3.5.3-7
docker build -t ${IMAGE} runtime-r35
docker push ${IMAGE}
 
IMAGE=tercen/runtime-r40:4.0.4-3
docker build -t ${IMAGE} runtime-r40
docker push ${IMAGE}

IMAGE=tercen/runtime-r40-slim:4.0.4-1
docker build -t ${IMAGE} runtime-r40-slim
docker push ${IMAGE}

IMAGE=tercen/runtime-matlab-image:r2020b-2
docker build -t ${IMAGE} matlab/runtime-matlab-image-r2020b
docker push ${IMAGE}
```
  
 

