ARG PARENT_IMAGE="python:3-slim-buster"
#---
# Place anything that is common to both the build and execution environment in base 
#
FROM ${PARENT_IMAGE} as base

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=America/Los_Angeles

RUN apt-get update && apt-get install -y \
    git \
    libusb-1.0-0 \
    usbutils \
    udev && \
    rm -rf /var/lib/apt/lists/*

FROM base as release-candidate

COPY 98-quickfeather.rules /etc/udev/rules.d/98-quickfeather.rules

WORKDIR /root

# install programmer
RUN git clone --recursive https://github.com/QuickLogic-Corp/TinyFPGA-Programmer-Application.git
RUN pip3 install tinyfpgab pyserial
# .bashrc contains: alias qfprog="python3 ~/TinyFPGA-Programmer-Application/tinyfpga-programmer-gui.py"
COPY .bashrc .bashrc

WORKDIR /root/work

SHELL [ "/bin/bash" ]

# ENTRYPOINT [ "/scripts/conda-user.sh" ]

FROM release-candidate as qfprog

# Below are some example commands to build and run a docker container.
# From within the dockerfiles directory in a terminal window type:
#
# docker build . -t qfprog 
# 
# In a directory containing a symbiflow project binary:
# docker run -it --rm -v $(pwd):/root/work qfprog bash
#
# The above will execute bash within the running container.  Any
# tools called thru bash will run within the container.
#
# To work with the hardware, you may need something like the following:
# 
# docker run --privileged -it --rm --device-cgroup-rule "c 166:* rwm" --device-cgroup-rule "c 189:* rwm" -v /dev/bus:/dev/bus:ro -v /dev/serial:/dev/serial:ro -v $(pwd):/root/work qfprog bash
#
# From the directory containing your bin file, the programming command will be something like:
# qfprog --port /dev/ttyACM0 --m4app qf_helloworldhw.bin
#
# I would like to get away from requiring --privileged.  Still working on it.