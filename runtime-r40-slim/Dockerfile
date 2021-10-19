FROM debian:bullseye-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
  openssl \
  libssl-dev \
  rustc \
  cargo && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y --no-install-recommends gnupg2 software-properties-common
#https://cloud.r-project.org/bin/linux/debian/#debian-buster-stable
COPY jranke.asc jranke.asc
RUN apt-key add jranke.asc
#RUN apt-key adv --keyserver keys.gnupg.net --recv-key 'E19F5F87128899B192B1A2C2AD5F960A256A04AF'
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/debian bullseye-cran40/'


ENV R_BASE_VERSION 4.0.4
#ENV TZ Europe/Paris5
#ENV DEBIAN_FRONTEND noninteractive
#ENV TERM linux
#RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y --no-install-recommends \
  r-base-core=${R_BASE_VERSION}-* \
  r-base-dev=${R_BASE_VERSION}-* && \
#  r-recommended=${R_BASE_VERSION}-* \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

RUN echo 'options(HTTPUserAgent = sprintf("R/%s R (%s)", getRversion(), paste(getRversion(), R.version["platform"], R.version["arch"], R.version["os"])))' >> /usr/lib/R/etc/Rprofile.site