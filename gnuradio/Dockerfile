FROM debian:stretch-slim

ARG DEBIAN_FRONTEND=noninteractive

ENV uhd_branch=master \
    gr_branch=maint

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        build-essential \
        cmake \
        git \
        libasound2-dev \
        libboost-all-dev \
        libfftw3-3 \
        libfftw3-dev \
        libgsl-dev \
        liblog4cpp5-dev \
        libqwt-dev \
        libqwt5-qt4 \
        libusb-1.0-0 \
        libusb-1.0-0-dev \
        libzmq3-dev \
        openssh-server \
        pkg-config \
        python-cairo-dev \
        python-cheetah \
        python-dev \
        python-gtk2 \
        python-lxml \
        python-numpy \
        python-pip \
        python-qt4 \
        python-qwt5-qt4 \
        python-setuptools \
        python-zmq \
        python3-pip \
        python3-setuptools \
        swig \
        x11-apps \
        ## Purge 
        && rm -rf /var/lib/apt/lists/* \
        ## Python Depencies
        && pip install --upgrade setuptools mako requests six numpy \
        && pip3 install --upgrade setuptools mako requests six numpy

WORKDIR /opt/
RUN git clone https://github.com/EttusResearch/uhd.git
WORKDIR /opt/uhd/host
RUN git checkout ${uhd_branch}

WORKDIR /opt/uhd/host/build
RUN cmake -DENABLE_B100=1 -DENABLE_B200=1 -DENABLE_E100=0 -DENABLE_E300=0 -DENABLE_EXAMPLES=1 -DENABLE_DOXYGEN=0 -DENABLE_MANUAL=0 -DENABLE_MAN_PAGES=0 -DENABLE_OCTOCLOCK=0 -DENABLE_USRP1=0 -DENABLE_USRP2=1 -DENABLE_UTILS=1 -DENABLE_X300=1 ../ \
    && make -j$(nproc) \
    && make install \
    && ldconfig

WORKDIR /opt/
RUN git clone --recursive https://github.com/gnuradio/gnuradio.git
WORKDIR /opt/gnuradio
RUN git checkout maint-3.7 
WORKDIR /opt/gnuradio/build
RUN cmake ../ \
    && make -j$(nproc) \
    && make install \   
    && ldconfig

RUN export DISPLAY=:0.0
CMD ["/bin/bash"]


