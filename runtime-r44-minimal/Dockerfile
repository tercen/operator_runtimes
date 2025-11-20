FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4.3 AS build

RUN installr -d -t "R-dev file git rust cargo" tercen/teRcenHttp@1.0.21 tercen/mtercen@1.0.8
RUN installr -d tercen/teRcenApi@0.13.3 tercen/teRcen@0.16.4

FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4.3

RUN apk add --no-cache jemalloc
ENV LD_PRELOAD=/usr/lib/libjemalloc.so.2

COPY --from=build /usr/local/lib/R/library /usr/local/lib/R/library
