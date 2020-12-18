FROM ubuntu:disco

# Install curl gnupg software-properties-common
RUN apt update && \
    apt install --no-install-recommends -y \
        build-essential \
        curl \
        gnupg \
        git \
        software-properties-common \
        zip \
        musl-tools

# Install python
RUN apt install --no-install-recommends -y \
        # Install python3-pip
        python3-pip \
        # Install python3.7
        python3.7 \
        python3.7-dev \
        python3.7-venv \
        # Install python3.8
        python3.8 \
        python3.8-dev \
        python3.8-venv

# Install setutools
RUN python3.8 -m pip install setuptools && \
    python3.7 -m pip install setuptools

# Install go 1.13
RUN add-apt-repository ppa:longsleep/golang-backports && \
    apt install --no-install-recommends -y golang-go

# Install rust nightly and target x86_64-unknown-linux-musl
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --profile minimal && \
    echo 'source $HOME/.cargo/env' >> ~/.bashrc && \
    /bin/bash -c "~/.cargo/bin/rustup default nightly" && \
    /bin/bash -c "~/.cargo/bin/rustup target add x86_64-unknown-linux-musl --toolchain=nightly"

# source ~/.bashrc
RUN /bin/bash -c "source ~/.bashrc"

# Install nodejs 12
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt install --no-install-recommends  -y nodejs

# Install serverless@1.60.5
RUN npm install -g serverless@1.60.5

# Install tox virtualenv
RUN pip3 install tox virtualenv

# Install docker
RUN apt install -y docker.io && service docker start

# cleanup
RUN apt-get --purge autoremove -y software-properties-common && \
    apt-get --purge autoremove -y gnupg && \
    rm -rf /var/cache/apt/lists

