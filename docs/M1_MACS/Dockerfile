# Docker build command to use with this
# docker build --platform linux/arm64 -t myshell-openvoice .

# Use the base image of Ubuntu with ARM support
FROM ubuntu:latest

# Avoid prompts from apt
ENV DEBIAN_FRONTEND=noninteractive

# Update the system and install necessary dependencies
RUN apt-get update && apt-get install -y \
    sudo \
    python3.8 \
    python3-pip \
    ffmpeg \
    git \
    wget

# Install Git
RUN apt-get install git

# Install Miniconda for ARM architecture
RUN mkdir -p /miniconda3
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh -O /miniconda3/miniconda.sh
RUN bash /miniconda3/miniconda.sh -b -u -p /miniconda3 
RUN rm /miniconda3/miniconda.sh

# Git clone using https
RUN git clone https://github.com/myshell-ai/OpenVoice.git

# Initialize conda for bash and zsh
RUN /miniconda3/bin/conda init bash && \
    /miniconda3/bin/conda init zsh

# Make RUN commands use `/bin/bash`:
SHELL ["/bin/bash", "-c"]

# Create a conda environment with Python 3.9
RUN /miniconda3/bin/conda init && \
    /miniconda3/bin/conda create -n openvoice python=3.9 -y && \
    echo "source /miniconda3/bin/activate openvoice" >> ~/.bashrc && \
    source /miniconda3/bin/activate openvoice && \
    cd OpenVoice && \
    pip install -e .

