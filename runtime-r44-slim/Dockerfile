FROM rocker/r-ver:4.4.3

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates wget curl gnupg2 build-essential pkg-config \
        libssl-dev libxml2-dev libcurl4-openssl-dev \
        cmake libgeos-dev libxt-dev libtiff5-dev libzstd-dev \
        libwebp-dev libglpk40 libglpk-dev;

RUN R -e "install.packages('remotes', repos='https://cran.r-project.org')" && \
    R -e "remotes::install_github('rstudio/renv@v1.1.3')" && \
    echo 'options(repos = c(TERCEN = "https://cran.tercen.com/api/v1/rlib/tercen", CRAN = "https://cran.tercen.com/api/v1/rlib/CRAN", Bioconductor = BiocManager::repositories()[names(BiocManager::repositories()) != "CRAN"]))' > /usr/local/lib/R/etc/Rprofile.site && \
    R -e "install.packages(c('dplyr', 'tidyr', 'data.table', 'BiocManager'))" && \
    R -e "install.packages('https://cran.tercen.com/api/v1/rlib/tercen/src/contrib/mtercen_1.0.9.tar.gz', repos=NULL, type='source')" && \
    R -e "install.packages('https://cran.tercen.com/api/v1/rlib/tercen/src/contrib/teRcenHttp_1.0.22.tar.gz', repos=NULL, type='source')" && \
    R -e "install.packages('https://cran.tercen.com/api/v1/rlib/tercen/src/contrib/tercen_0.16.1.tar.gz', repos=NULL, type='source')" && \
    R -e "install.packages('https://cran.tercen.com/api/v1/rlib/tercen/src/contrib/tercenApi_0.13.3.tar.gz', repos=NULL, type='source')"

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]