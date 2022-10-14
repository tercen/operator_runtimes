FROM tercen/runtime-r40:4.0.4-4

RUN apt-get install -y  cmake

ENTRYPOINT ["/usr/local/bin/R" , "--no-save" , "--no-restore" , "--no-environ" , "--slave" ]
