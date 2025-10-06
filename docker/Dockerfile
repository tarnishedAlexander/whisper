FROM nvidia/cuda:11.2.2-devel-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    lsb-release \
    gnupg2 \
    curl \
    git \
    nano \
    vim \
    build-essential \
    python3-pip \
    python3-dev \
    tmux \
    libgl1-mesa-glx \
    libegl1-mesa \
    libxrandr2 \
    libxss1 \
    libxcursor1 \
    libxcomposite1 \
    libasound2 \
    libxi6 \
    libxtst6 \
    wget \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*

# Rust for tiktoken
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/root/.cargo/bin:$PATH"

ENV CONDA_DIR /opt/anaconda
RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/anaconda && \
    rm ~/anaconda.sh

ENV PATH=$CONDA_DIR/bin:$PATH

# Initialize conda
RUN /opt/anaconda/bin/conda init bash

# Python 3.9 and dependencies
RUN /opt/anaconda/bin/conda create -n myenv python=3.9.9 -y && \
    /opt/anaconda/bin/conda run -n myenv pip install setuptools-rust && \
    /opt/anaconda/bin/conda run -n myenv pip install torch==1.10.1+cu112 -f https://download.pytorch.org/whl/torch_stable.html && \
    /opt/anaconda/bin/conda run -n myenv pip install openai-whisper tiktoken

WORKDIR /root/wishpearWS

EXPOSE 8888