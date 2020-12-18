FROM amazonlinux:1

RUN yum -y update && yum install -y \
   # C++ to bootstrap
   gcc-c++ gcc git make wget tar.x86_64 \
   # python3
   python36* \
   # Dependencies
   boost-static glibc-static \
   # gcc-9.2 pre-requisites
   bzip2 bison yacc flex

# Install CMake 3.13
RUN version=3.13 && build=5 \
   && mkdir ~/temp-cmake \
   && cd ~/temp-cmake \
   && wget https://cmake.org/files/v$version/cmake-$version.$build-Linux-x86_64.sh \
   && mkdir /opt/cmake \
   && sh cmake-$version.$build-Linux-x86_64.sh --prefix=/opt/cmake --skip-license \
   && for filename in /opt/cmake/bin/*; do echo Registering $filename; ln -fs $filename /usr/local/bin/`basename $filename`; done \
   && rm -f cmake-$version.$build-Linux-x86_64.sh

# # Install gcc-9.2
# RUN wget ftp://ftp.mirrorservice.org/sites/sourceware.org/pub/gcc/releases/gcc-9.2.0/gcc-9.2.0.tar.gz \
#    && tar xfz gcc-9.2.0.tar.gz \
#    && cd gcc-9.2.0 \
#    && ./contrib/download_prerequisites \
#    && ./configure --disable-multilib --enable-languages=c,c++ --with-pic \
#    && make -j$(nproc) \
#    && make install \
#    && cd .. && rm -rf gcc-9.2.0.tar.gz gcc-9.2.0 \
#    && chmod +x /usr/local/libexec/gcc/x86_64-pc-linux-gnu/9.2.0/cc1plus

# Install gcc-7.2
RUN yum install -y gcc72 gcc72-c++

# Update path so it uses gcc 9.2
ENV PATH="/usr/local/bin:${PATH}"

# Install zlib
RUN wget -O zlib.tar.gz 'https://zlib.net/fossils/zlib-1.2.8.tar.gz' \
   && tar xvzf zlib.tar.gz \
   && cd zlib-1.2.8 \
   && ./configure --static --archs=-fPIC \
   && make -j$(nproc) \
   && make install \
   && cd .. && rm -rf zlib*

RUN wget -O boost.tar.gz 'https://sourceforge.net/projects/boost/files/boost/1.65.0/boost_1_65_0.tar.gz/download' \
   && tar -xvzf boost.tar.gz \
   && mkdir boost_output \
   && cd boost_1_65_0 \
   && ./bootstrap.sh --prefix=/boost_output --with-python=python3.6 --with-libraries=python,program_options,system,thread,test,chrono,date_time,atomic \
   && sed -i 's|using\ python\ \:\ 3.6\ \:\ /usr|using\ python\ \:\ 3.6\ \:\ /usr/bin/python3.6\ :\ /usr/include/python3.6m\ :\ /usr/lib64|' project-config.jam \
   && ./bjam -j$(nproc) cxxflags=-fPIC cflags=-fPIC -a install \
   && cp -f /boost_output/lib/libboost_system.a /boost_output/lib/libboost_program_options.a /usr/lib64 \
   && cd .. && rm -rf boost_1_65_0 boost.tar.gz

ENV BOOSTROOT=/boost_output

# install vowpalwabbit
RUN pip-3.6 install vowpalwabbit==8.8.1

WORKDIR /tmp
RUN mkdir -p build/lib && mkdir -p build/python && \
   # shared libraries
   ldd /usr/local/lib64/python3.6/site-packages/pylibvw.so  | grep "=> /" | awk '{print $3}' | xargs -I '{}' cp -v '{}' build/lib/ && \
   cp /usr/local/lib64/python3.6/site-packages/pylibvw.so build/python && \
   # python package
   cp -r /usr/local/lib64/python3.6/site-packages/vowpalwabbit build/python

