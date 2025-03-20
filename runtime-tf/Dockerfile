FROM tensorflow/tensorflow:2.17.0-gpu

RUN apt-get update && apt-get install --no-install-recommends -y build-essential \
	ca-certificates \
	sudo \
	git \
	jq \
	libbz2-dev \
	libffi-dev \
	libreadline-dev \
	libssl-dev \
	libsqlite3-dev \
	liblzma-dev \
	zlib1g-dev \
	wget \
	curl \
	llvm \
	libncurses5-dev \
	libncursesw5-dev \
	xz-utils \
	tk-dev

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.85.0

#    https://static.rust-lang.org/rustup/archive/1.28.0/x86_64-unknown-linux-gnu/rustup-init.sha256

RUN set -eux; \
    dpkgArch="$(dpkg --print-architecture)"; \
    rustArch="x86_64-unknown-linux-gnu"; \
    rustupSha256="c8d03f559a2335693379e1d3eaee76622b2a6580807e63bcd61faea709b9f664"; \
    RUSTUP_VERSION="1.28.0"; \
    url="https://static.rust-lang.org/rustup/archive/${RUSTUP_VERSION}/${rustArch}/rustup-init"; \
    wget "$url"; \
    echo "${rustupSha256} *rustup-init" | sha256sum -c -; \
    chmod +x rustup-init; \
#    ./rustup-init -y --no-modify-path --profile minimal --default-toolchain $RUST_VERSION --default-host ${rustArch}; \
    ./rustup-init -y --no-modify-path --profile minimal --default-toolchain $RUST_VERSION; \
    rm rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup --version; \
    cargo --version; \
    rustc --version;

ENV PYTHONPATH="/usr/bin/python"

