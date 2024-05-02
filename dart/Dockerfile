FROM debian:bullseye-slim

RUN apt-get update && \
  apt-get install -y --no-install-recommends apt-transport-https wget gnupg2 software-properties-common git &&\
  wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/dart.gpg &&\
  echo 'deb [signed-by=/usr/share/keyrings/dart.gpg arch=amd64] https://storage.googleapis.com/download.dartlang.org/linux/debian stable main' | tee /etc/apt/sources.list.d/dart_stable.list &&\
  apt-get update &&\
  apt-get install dart=3.3.1-1 &&\
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENV PATH="$PATH":/usr/lib/dart/bin
