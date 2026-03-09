FROM docker.io/tercen/runtime-r35:3.5.3-8

USER root

# Switch to archive repos (Debian Stretch is EOL)
RUN sed -i 's|deb.debian.org/debian|archive.debian.org/debian|g' /etc/apt/sources.list && \
    sed -i 's|security.debian.org/debian-security|archive.debian.org/debian-security|g' /etc/apt/sources.list && \
    sed -i '/stretch-updates/d' /etc/apt/sources.list

# Install jemalloc for improved memory allocation
RUN apt-get update && apt-get install -y --allow-unauthenticated libjemalloc-dev libjemalloc1
ENV LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.1

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]
