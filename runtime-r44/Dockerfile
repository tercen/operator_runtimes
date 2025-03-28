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

RUN apt-get update && apt-get install -y cmake libgeos-dev libxt-dev libtiff5-dev libzstd-dev libwebp-dev libglpk40 libglpk-dev

ENV RENV_VERSION v1.1.3
RUN R -e "install.packages('remotes', repos = c(CRAN = 'https://cran.r-project.org'))"
RUN R -e "remotes::install_github('rstudio/renv@${RENV_VERSION}')"

RUN echo 'options(repos=c(TERCEN="https://cran.tercen.com/api/v1/rlib/tercen"))' >> /usr/local/lib/R/etc/Rprofile.site¨

RUN R -e "install.packages('tercen/teRcenHttp', ref = '1.0.21')"
RUN R -e "install.packages('tercen/mtercen', ref = '1.0.8')"
RUN R -e "install.packages('tercen/tercenApi', ref = '0.13.3')"
RUN R -e "install.packages('tercen/teRcen', ref = '0.13.17')"

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]
