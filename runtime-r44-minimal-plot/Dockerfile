FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4.3 AS build

# Install Tercen packages using installr for efficiency
RUN installr -d -t "R-dev file git rust cargo" tercen/teRcenHttp@1.0.21 tercen/mtercen@1.0.8
RUN installr -d tercen/teRcenApi@0.13.3 tercen/teRcen@0.16.4

# Install essential R packages for plotting
RUN installr -d data.table dtplyr Cairo

FROM ghcr.io/r-hub/r-minimal/r-minimal:4.4.3

COPY --from=build /usr/local/lib/R/library /usr/local/lib/R/library

# Install minimal graphics dependencies for plotting (no X11 needed)
RUN apk add --no-cache \
    cairo \
    cairo-dev \
    font-dejavu \
    fontconfig \
    libpng \
    libpng-dev \
    libjpeg-turbo \
    libjpeg-turbo-dev \
    pango \
    pango-dev

# No X11 setup needed - Cairo handles graphics directly
ENTRYPOINT ["/usr/local/bin/R", "--no-save", "--no-restore", "--no-environ", "--slave"]