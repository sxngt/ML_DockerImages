ARG UBUNTU_VERSION=18.04
ARG CUDA_VERSION=11.4.3
ARG CUDA=11.4
ARG CUDNN_VERSION=8

FROM nvidia/cuda:${CUDA_VERSION}-cudnn${CUDNN_VERSION}-devel-ubuntu${UBUNTU_VERSION}
LABEL maintainer "http://ysh8614.github.io"

# 이상한거 안깔거임
RUN rm -f /etc/apt/sources.list.d/*.list

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


ENV LANG C.UTF-8
RUN curl -o /tmp/miniconda.sh -sSL http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    chmod +x /tmp/miniconda.sh && \
    bash /tmp/miniconda.sh -bfp /usr/local && \
    rm /tmp/miniconda.sh
RUN conda update -y conda

# ssh 가끔은.. 필요하지 않을까.. 22포트호스팅 꼭 해줍시다 
RUN echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config && \
    echo "PermitEmptyPasswords yes" >> /etc/ssh/sshd_config && \
    echo "UsePAM no" >> /etc/ssh/sshd_config

ARG PYTHON_VERSION=3.8
ARG CONDA_ENV_NAME=tf_env
ARG TF_VERSION=2.4.0

# conda 세팅
RUN conda create -n $CONDA_ENV_NAME python=$PYTHON_VERSION
ENV PATH /usr/local/envs/$CONDA_ENV_NAME/bin:$PATH
RUN echo "source activate ${CONDA_ENV_NAME}" >> ~/.bashrc

# 주피터 쓰자... code-server는 쓰는거 아닙니다 진짜로
RUN source activate ${CONDA_ENV_NAME} && \
    conda install -c conda-forge jupyterlab && \
    jupyter serverextension enable --py jupyterlab --sys-prefix

# Install the packages
RUN source activate ${CONDA_ENV_NAME} && \
    python -m pip install --no-cache-dir tensorflow-gpu==${TF_VERSION}