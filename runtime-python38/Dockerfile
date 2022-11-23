FROM debian:bullseye-slim

USER root 

# RUN apt-get update && apt-get install -y --no-install-recommends \
#   openssl \
#   libssl-dev \
#   rustc \
#   cargo && \
#   apt-get clean && \
#   rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y python3.8 python3.8-dev python3.8-pip python3.8-venv python3.8-wheel git
RUN pip3 install wheel
RUN pip3 install git+https://github.com/tercen/tercen_python_client@0.0.1


ENTRYPOINT ["python3"]




