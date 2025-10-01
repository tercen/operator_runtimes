FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4.3 AS build

RUN installr -d -t "R-dev file git rust cargo" tercen/teRcenHttp@1.0.21 tercen/mtercen@1.0.8
RUN installr -d tercen/teRcenApi@0.13.3 tercen/teRcen@0.16.2

FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4.3

COPY --from=build /usr/local/lib/R/library /usr/local/lib/R/library

RUN installr -d R6 yaml dplyr data.table dtplyr tibble yaml uuid base64enc