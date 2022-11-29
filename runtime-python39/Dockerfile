FROM debian:bullseye-slim

USER root 

# RUN apt-get update && apt-get install -y --no-install-recommends \
#   openssl \
#   libssl-dev \
#   rustc \
#   cargo && \
#   apt-get clean && \
#   rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y python3 python3-dev python3-pip python3-venv git jq
RUN pip3 install wheel
# Will install wheel defined by the version in setup.py
# Wheel created with python3 setup.py bdist_wheel
# RUN pip3 install --ignore-installed git+https://github.com/tercen/tercen_python_client

# Requirementes generated running pigar in tercen (source) folder.
# RUN pip3 install --no-cache -r https://raw.githubusercontent.com/tercen/tercen_python_client/master/requirements.txt

#/master/tercen-0.0.1-py3-none-any.whl


#ENTRYPOINT ["python3"]




