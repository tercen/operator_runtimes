FROM debian:bullseye-slim as builder

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -q update && \
    apt-get install -q -y --no-install-recommends \
      xorg \
      unzip \
      wget \
      curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY installer_input.txt /installer_input.txt

RUN mkdir /mcr-install && \
    mkdir /opt/mcr && \
    cd /mcr-install && \
    wget --no-check-certificate -q https://ssd.mathworks.com/supportfiles/downloads/R2020b/Release/5/deployment_files/installer/complete/glnxa64/MATLAB_Runtime_R2020b_Update_5_glnxa64.zip && \
    unzip -q MATLAB_Runtime_R2020b_Update_5_glnxa64.zip && \
    rm -f MATLAB_Runtime_R2020b_Update_5_glnxa64.zip && \
    ./install -inputFile /installer_input.txt && \
    cd / && \
    rm -rf mcr-install

#RUN mkdir /mcr-install && \
#    mkdir /opt/mcr
#
#RUN echo hey
#COPY MATLAB_Runtime_R2020b_Update_5_glnxa64.zip /mcr-install/MATLAB_Runtime_R2020b_Update_5_glnxa64.zip
#
#COPY installer_input.txt /mcr-install/installer_input.txt
#
#RUN cd /mcr-install && \
#    unzip -q MATLAB_Runtime_R2020b_Update_5_glnxa64.zip && \
#    rm -f MATLAB_Runtime_R2020b_Update_5_glnxa64.zip && \
#    ./install -inputFile /mcr-install/installer_input.txt && \
#    cd / && \
#    rm -rf mcr-install

FROM tercen/runtime-r40-slim:4.0.4-1

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -q update && \
    apt-get install -q -y --no-install-recommends \
      xorg && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY --from=builder /opt/mcr/v99 /opt/mcr/v99

#ENV LD_LIBRARY_PATH .:/opt/mcr/v99/runtime/glnxa64:/opt/mcr/v99/bin/glnxa64:/opt/mcr/v99/sys/os/glnxa64:/opt/mcr/v99/sys/opengl/lib/glnxa64:/opt/mcr/v99/extern/bin/glnxa64

#ENV XAPPLRESDIR /etc/X11/app-defaults