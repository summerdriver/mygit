FROM ubuntu:20.04

ARG VERSION
ARG BUILDNUM
ARG SYMBOL

ENV VERSION ${VERSION}
ENV BUILDNUM ${BUILDNUM}

RUN DEBIAN_FRONTEND=noninteractive apt-get update --fix-missing && apt-get install -y \
    wget \
    curl \
    gnupg \
    jq \
    procps \
    python3 \
    python3-distutils \
    unzip \
    git \
    make \
    cmake \
    gcc \
    xz-utils \
    lbzip2 \
    tar \
    gzip \
    net-tools \
    rsync \
  && rm -rf /var/lib/apt/lists/* 

  #&& useradd -Urs /usr/sbin/nologin "${SYMBOL}"
		
#instal go env
RUN wget https://go.dev/dl/go1.19.1.linux-amd64.tar.gz \
     && tar -zxvf go1.19.1.linux-amd64.tar.gz \
     && mv go /usr/lib/go

RUN apt-get  update --fix-missing && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y git build-essential libgmp-dev libssl-dev zlib1g-dev solc && \
    git clone --branch ${VERSION} https://github.com/ethereum/go-ethereum.git && \
    cd go-ethereum && \
    make geth && \
    mv build/bin/geth /usr/local/bin/ && \
    cd .. && \
    rm -rf go-ethereum && \
    apt-get -y autoremove --purge build-essential git

EXPOSE 8545 8546 8547 30303 30303/udp

ENTRYPOINT ["geth"]

ARG COMMIT="$(git rev-parse HEAD)"

LABEL commit="$COMMIT" version="$VERSION" buildnum="$BUILDNUM"

#CMD ["geth", "--goerli", "--rpc", "--rpcaddr", "0.0.0.0"]
