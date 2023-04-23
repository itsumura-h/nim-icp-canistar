FROM ubuntu:22.04

# prevent timezone dialogue
ENV DEBIAN_FRONTEND=noninteractive

RUN apt update --fix-missing && \
    apt upgrade -y
RUN apt install -y --fix-missing \
        make \
        xz-utils \
        ca-certificates \
        vim \
        wget \
        curl \
        git \
        libunwind-dev

# LLVM
RUN apt install -y clang lldb lld

WORKDIR /root
# ==================== ICP ====================
RUN sh -ci "$(curl -fsSL https://sdk.dfinity.org/install.sh)"
RUN dfx --version

# ==================== WASI SDK ====================
ENV WASI_VERSION "14"
ENV WASI_VERSION_FULL "$WASI_VERSION.0"
RUN wget https://github.com/WebAssembly/wasi-sdk/releases/download/wasi-sdk-${WASI_VERSION}/wasi-sdk-${WASI_VERSION_FULL}-linux.tar.gz
RUN tar xvf wasi-sdk-${WASI_VERSION_FULL}-linux.tar.gz
RUN rm wasi-sdk-${WASI_VERSION_FULL}-linux.tar.gz

ENV WASI_SDK_PATH "/root/wasi-sdk-${WASI_VERSION_FULL}"
RUN echo $WASI_SDK_PATH

# ==================== Chico ====================
RUN clang --version -v
RUN git clone https://github.com/ICPorts-labs/chico.git
ENV CHICO_PATH "/root/chico/src"

# ==================== Nim ====================
WORKDIR /root
ARG VERSION="1.6.12"
RUN wget --inet4-only https://nim-lang.org/download/nim-${VERSION}-linux_x64.tar.xz && \
    tar -Jxf nim-${VERSION}-linux_x64.tar.xz && \
    rm -f nim-${VERSION}-linux_x64.tar.xz && \
    mv nim-${VERSION} .nimble
ENV PATH $PATH:/root/.nimble/bin
RUN nim -v

# ==================== Rust ====================
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y

WORKDIR /application
