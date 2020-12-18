FROM ubuntu:20.04
RUN apt update
RUN apt install -y libboost-dev libboost-program-options-dev libboost-system-dev libboost-thread-dev libboost-math-dev libboost-test-dev libboost-python-dev zlib1g-dev cmake python3 python3-pip
RUN apt install -y git

WORKDIR /tmp
RUN git clone https://github.com/VowpalWabbit/vowpal_wabbit.git

WORKDIR /tmp/vowpal_wabbit/
RUN mkdir build && cd build && cmake .. -DSTATIC_LINK_VW=ON && make vw-bin -j `nproc`

WORKDIR /
RUN mkdir -p /artifacts/bin && cp /tmp/vowpal_wabbit/build/vowpalwabbit/vw /artifacts/bin/

