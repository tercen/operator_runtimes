FROM bioconductor/bioconductor_docker:RELEASE_3_15

USER root

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.40.0

RUN set -eux; \
    dpkgArch="$(dpkg --print-architecture)"; \
    case "${dpkgArch##*-}" in \
        amd64) rustArch='x86_64-unknown-linux-gnu'; rustupSha256='7d610882f67ec4e53a56a8177d7862501a043f80be7ba13b6e325cc9921f23b8' ;; \
        armhf) rustArch='armv7-unknown-linux-gnueabihf'; rustupSha256='3bc5c5fff32113dc20284bd605eb2a6f7070de0c69c742b00b0ca4511a8fbc4c' ;; \
        arm64) rustArch='aarch64-unknown-linux-gnu'; rustupSha256='cff57e155046439896004d7eb66fcbebe436f00298b1dacef426aef0a109a866' ;; \
        i386) rustArch='i686-unknown-linux-gnu'; rustupSha256='2d82bf50439c6ec74af4a5642a004fe8915921b52d3ec54032e0dc10476718c1' ;; \
        *) echo >&2 "unsupported architecture: ${dpkgArch}"; exit 1 ;; \
    esac; \
    url="https://static.rust-lang.org/rustup/archive/1.18.1/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path --default-toolchain $RUST_VERSION; \
    rm rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup --version; \
    cargo --version; \
    rustc --version;

RUN apt-get update && apt-get install -y python3 python3-dev python3-pip python3-venv python3-wheel
RUN pip3 install wheel
# needed for flowCut
RUN apt-get install -y libgeos-dev libxt-dev libtiff5-dev libzstd-dev libwebp-dev libglpk-dev

# Global site-wide config
RUN  echo 'options(repos=c(TERCEN="https://cran.tercen.com/api/v1/rlib/tercen",\
     CRAN="https://cran.tercen.com/api/v1/rlib/CRAN", \
     BioCsoft="https://cran.tercen.com/api/v1/rlib/BioCsoft-3.12", \
     BioCann="https://cran.tercen.com/api/v1/rlib/BioCann-3.12", \
     BioCexp="https://cran.tercen.com/api/v1/rlib/BioCexp-3.12", \
     BioCworkflows="https://cran.tercen.com/api/v1/rlib/BioCworkflows-3.12" \
     ))' >> /usr/local/lib/R/etc/Rprofile.site

RUN R -e "BiocManager::install('MetaCyto')"
RUN R -e "BiocManager::install('openCyto')"
RUN R -e "BiocManager::install('diffcyt')"
RUN R -e "BiocManager::install('CATALYST')"

RUN R -e "remotes::install_github('ghar1821/TrackSOM', ref = 'v0.1.0')"
RUN R -e "remotes::install_github('RGLab/scamp', ref='v0.5.0')"
RUN R -e "remotes::install_github('RGLab/FAUST', ref='f900614')"

RUN R -e "remotes::install_github('tercen/tim', ref = '0.0.20')"
RUN R -e "remotes::install_github('tercen/teRcen', ref='0.12.7')"

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]
