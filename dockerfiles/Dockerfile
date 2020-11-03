FROM python:3-slim-buster

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=America/Los_Angeles

RUN apt-get update && apt-get install -y \
    git \
    libusb-1.0-0 \
    usbutils \
    udev && \
    rm -rf /var/lib/apt/lists/*

COPY 98-quickfeather.rules /etc/udev/rules.d/98-quickfeather.rules

WORKDIR /root

# install programmer
RUN git clone --recursive https://github.com/QuickLogic-Corp/TinyFPGA-Programmer-Application.git
RUN pip3 install tinyfpgab pyserial
# .bashrc contains: alias qfprog="python3 ~/TinyFPGA-Programmer-Application/tinyfpga-programmer-gui.py"
COPY .bashrc .bashrc

WORKDIR /root/work

# Below are some example commands to build and run a docker container.
# From within the dockerfiles directory in a terminal window type:
#
# docker build . -t qfprog 
# 
# In a directory containing a quickfeather symbiflow project binary,
# to work with the hardware, you may need to run the container 
# privileged with a command like the following:
# 
# docker run --privileged -it --rm -v $(pwd):/root/work qfprog bash
#
# The above will execute bash within the running container.  Any
# tools called through bash will run within the container.
#
# From the directory containing your bin file, the programming command will be something like:
# qfprog --port /dev/ttyACM0 --m4app qf_helloworldhw.bin
#
# There is intentionally no user and no entrypoint in this container image, 
# which makes it easier to re-use this image as the base of a new container image.