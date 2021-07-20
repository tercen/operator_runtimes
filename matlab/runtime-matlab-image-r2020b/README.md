```shell
# Build matlab runtime image
docker build -t tercen/mcr:R2020b docker/runtime_R2020b
docker push tercen/mcr:R2020b

docker run -it --rm tercen/mcr:R2020b-light bash

docker tag matlabruntime/r2020b/release/update5/c006000000000000 tercen/mcr:R2020b
```