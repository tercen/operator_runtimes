FROM python:3.14.0-alpine3.22

RUN apk add --no-cache jemalloc

ENV LD_PRELOAD=/usr/lib/libjemalloc.so.2

# Optional: Set jemalloc tuning parameters
#ENV MALLOC_CONF=background_thread:true,metadata_thp:auto