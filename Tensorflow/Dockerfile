ARG UBUNTU_VERSION=18.04
ARG CUDA_VERSION=11.4.3
ARG CUDA=11.4
ARG CUDNN_VERSION=8

FROM nvidia/cuda:${CUDA_VERSION}-cudnn${CUDNN_VERSION}-devel-ubuntu${UBUNTU_VERSION}

LABEL maintainer "http://ysh8614.github.io"

# 이상한거 안깔거임

#이거 안하면 터미널이 자꾸 말걸어서 피곤함
ARG DEBIAN_FRONTEND=noninteractive
# 필요한거
RUN apt-get update -y && apt-get install -y --no-install-recommends\
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
    python3-openssl && \
    rm -rf /var/lib/apt/lists/*

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
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install miniconda3
# Install miniconda
# - python에서 ML/DL을 사용할 것이기 때문에 여러모로 편리한 miniconda를 설치합니다.
# - 환경변수도 함께 지정해줍니다.
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH
RUN apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion
RUN wget --quiet https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc
RUN apt-get install -y curl grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

ARG PYTHON_VERSION=3.8
ARG TF_VERSION=2.4.0

RUN conda create -n tf python=${PYTHON_VERSION}
RUN conda activate tf

# 주피터 쓰자... code-server는 쓰는거 아닙니다 진짜로
RUN python -m pip install --upgrade pip && \
    pip install jupyter && \
    pip install jupyterlab

# Install the packages
RUN python -m pip install --no-cache-dir tensorflow-gpu==${TF_VERSION}