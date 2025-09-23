#FROM rocker/tidyverse:4.4.3
FROM tercen/tidyverse:4.4.3

USER root

ENV RENV_VERSION=v1.1.3 \
    RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.85.0 \
    DEBIAN_FRONTEND=noninteractive

# Install build-time dependencies and Rust, install R packages, then remove build tools and caches to reduce image size
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates wget curl gnupg2 build-essential pkg-config \
        libssl-dev libxml2-dev libcurl4-openssl-dev \
        cmake libgeos-dev libxt-dev libtiff5-dev libzstd-dev libwebp-dev libglpk40 libglpk-dev; \
    dpkgArch="$(dpkg --print-architecture)"; \
    rustArch="x86_64-unknown-linux-gnu"; \
    RUSTUP_VERSION="1.28.0"; \
    rustupSha256="c8d03f559a2335693379e1d3eaee76622b2a6580807e63bcd61faea709b9f664"; \
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

RUN R -e "install.packages('remotes', repos = c(CRAN = 'https://cran.r-project.org'))" \
    R -e "install.packages(c('dplyr', 'tidyr', 'data.table', 'BiocManager'), repos = c(CRAN = 'https://cran.r-project.org'))" \
    R -e "remotes::install_github('rstudio/renv@${RENV_VERSION}')" \
    echo 'options(repos=c(TERCEN="https://cran.tercen.com/api/v1/rlib/tercen", \
     CRAN="https://cran.tercen.com/api/v1/rlib/CRAN", BiocManager::repositories()[names(BiocManager::repositories()) != "CRAN"]))' >> /usr/local/lib/R/etc/Rprofile.site \
    R -e "install.packages('tercen/teRcenHttp', ref = '1.0.21')" \
    R -e "install.packages('tercen/mtercen', ref = '1.0.8')" \
    R -e "install.packages('tercen/tercenApi', ref = '0.13.3')" \
    R -e "install.packages('tercen/teRcen', ref = '0.13.17')"

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]
