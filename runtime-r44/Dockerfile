FROM rocker/tidyverse:4.4.3

USER root

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.85.0

RUN set -eux; \
    dpkgArch="$(dpkg --print-architecture)"; \
    rustArch="x86_64-unknown-linux-gnu"; \
    rustupSha256="c8d03f559a2335693379e1d3eaee76622b2a6580807e63bcd61faea709b9f664"; \
    RUSTUP_VERSION="1.28.0"; \
    url="https://static.rust-lang.org/rustup/archive/${RUSTUP_VERSION}/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path --profile minimal --default-toolchain $RUST_VERSION; \
    rm rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup --version; \
    cargo --version; \
    rustc --version;

RUN apt-get update && apt-get install -y cmake libgeos-dev libxt-dev libtiff5-dev libzstd-dev libwebp-dev

RUN R -e "remotes::install_github('tercen/teRcenHttp', ref = '1.0.21', upgrade = FALSE, force = TRUE, dependencies = c('Depends', 'Imports', 'Remotes'))"
RUN R -e "remotes::install_github('tercen/mtercen', ref = '1.0.8', upgrade = FALSE, force = TRUE, dependencies = c('Depends', 'Imports', 'Remotes'))"
RUN R -e "remotes::install_github('tercen/tercenApi', ref = '0.13.3', upgrade = FALSE, force = TRUE, dependencies = c('Depends', 'Imports', 'Remotes'))"
RUN R -e "remotes::install_github('tercen/teRcen', ref = '0.13.12', upgrade = FALSE, force = TRUE, dependencies = c('Depends', 'Imports', 'Remotes'))"

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]
