FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4 AS build

RUN installr -d -t "R-dev file git rust cargo" tercen/teRcenHttp@1.0.21 tercen/mtercen@1.0.8
RUN installr -d tercen/teRcenApi@0.13.3 tercen/teRcen@0.16.0

FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4

COPY --from=build /usr/local/lib/R/library /usr/local/lib/R/library
#teRcenApi
RUN installr -d R6 yaml
#teRcen
RUN installr -d R6 dplyr httr  tibble  yaml  openssl rstudioapi  uuid base64enc