ARG UBUNTU_VERSION=22.04
ARG CUDA_VERSION=12.2.2
ARG CUDA=12.2
ARG CUDNN_VERSION=8

FROM nvidia/cuda:${CUDA_VERSION}-cudnn${CUDNN_VERSION}-devel-ubuntu${UBUNTU_VERSION}

LABEL maintainer "http://sxngt.github.io"

# 이상한거 안깔거임

#이거 안하면 터미널이 자꾸 말걸어서 피곤함
ARG DEBIAN_FRONTEND=noninteractive
# 필요한거
# Set environment variables
ENV PYENV_ROOT /root/.pyenv
ENV PATH $PYENV_ROOT/bin:$PYENV_ROOT/shims:$PATH

# Install dependencies
RUN apt-get update -y && apt-get install -y --no-install-recommends \
    vim \
    curl \
    apt-utils \
    ssh \
    tree \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
    make \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    wget \
    llvm \
    libncursesw5-dev \
    xz-utils \
    tk-dev \
    libxml2-dev \
    libxmlsec1-dev \
    libffi-dev \
    liblzma-dev \
    python3-openssl \
    && rm -rf /var/lib/apt/lists/*

# 시간설정
ENV TZ=Asia/Seoul
RUN sudo ln -snf /usr/share/zoneinfo/$TZ /etc/localtime

# ssh 가끔은.. 필요하지 않을까.. 22포트호스팅 꼭 해줍시다 
RUN echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config && \
    echo "PermitEmptyPasswords yes" >> /etc/ssh/sshd_config && \
    echo "UsePAM no" >> /etc/ssh/sshd_config

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

# Install Dependencies of Miniconda
RUN apt-get update --fix-missing && \
    apt-get install -y wget bzip2 curl git && \
    apt-get clean

# Install miniconda3
# Install miniconda
# - 환경변수도 함께 지정해줍니다.
RUN curl https://pyenv.run | bash

# Install specific Python version and set it as global
RUN pyenv install 3.9.12
RUN pyenv global 3.9.12

RUN curl -fsSL https://code-server.dev/install.sh | sh


ENV LANG ko_KR.UTF-8
RUN apt-get update

RUN pip install --upgrade pip

# 주피터 쓰자... code-server는 쓰는거 아닙니다 진짜로
