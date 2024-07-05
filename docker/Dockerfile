# Use a base image with Python and necessary dependencies
FROM ubuntu:20.04

# Set the environment variable
ENV DEBIAN_FRONTEND=noninteractive

# Set the working directory
WORKDIR /opt

# Install necessary tools
RUN apt-get update && apt-get install -y \
    samtools \
    minimap2 \
    tabix \
    autoconf \
    bzip2 \
    g++


RUN apt install -yq --no-install-recommends \
    ca-certificates build-essential cmake curl wget git git-lfs \
    zlib1g-dev libbz2-dev liblzma-dev libncurses5-dev libcurl4-gnutls-dev \
    libssl-dev libffi-dev \
    libreadline8 libreadline-dev sqlite3 libsqlite3-dev file \
    python3-all-dev python3-venv python3-pip python3-setuptools \
    && update-alternatives --install /usr/bin/python python /usr/bin/python3 10

# Install bgzip
RUN git clone --recursive https://github.com/samtools/htslib

WORKDIR /opt/htslib
RUN autoreconf -i
RUN ./configure 
RUN make 
RUN make install

# Copy requirements file
COPY docker/requirements.txt .
# Install Python packages
RUN pip install --upgrade pip
RUN pip install -r requirements.txt
RUN pip install pyabpoa

WORKDIR /opt
# Install medaka
RUN git clone --recursive https://github.com/nanoporetech/medaka
WORKDIR /opt/medaka
RUN make install_root

# Add medaka to PATH
ENV PATH="/opt/medaka/install_root/bin:${PATH}"
# Add command to run the container
CMD ["medaka"]

# build the container
# docker build -f docker/Dockerfile -t medaka-test .