FROM rocker/r-ver:4.4.3

USER root

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates wget curl gnupg2 build-essential pkg-config \
        libssl-dev libxml2-dev libcurl4-openssl-dev \
        cmake libgeos-dev libxt-dev libtiff5-dev libzstd-dev \
        libwebp-dev libglpk40 libglpk-dev;

RUN mkdir -p /usr/local/lib/R/site-library && \
    chown -R root:staff /usr/local/lib/R/site-library && \
    R -q -e "install.packages(c('remotes', 'renv', 'dplyr', 'tidyr', 'data.table', 'BiocManager'), lib='/usr/local/lib/R/site-library')" && \
    echo 'options(repos = c(TERCEN = "https://cran.tercen.com/api/v1/rlib/tercen", CRAN = "https://p3m.dev/cran/__linux__/noble/2025-04-10", Bioconductor = BiocManager::repositories()[names(BiocManager::repositories()) != "CRAN"]))' > /usr/local/lib/R/etc/Rprofile.site && \
    R -e "install.packages(c('mtercen', 'teRcenHttp', 'tercen', 'tercenApi'), lib='/usr/local/lib/R/site-library')" && \
    strip /usr/local/lib/R/site-library/*/libs/*.so

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]
