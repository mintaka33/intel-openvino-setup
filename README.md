# intel-openvino-setup

## 0. reference

https://github.com/opencv/dldt/blob/2019/inference-engine/README.md

## 1. source code

https://github.com/opencv/dldt

```bash
cd WORK_DIR
git clone https://github.com/opencv/dldt.git

cd dldt/inference-engine/thirdparty
git clone https://github.com/opencv/ade.git
```

## 2. install dependencies

https://github.com/opencv/dldt/blob/2019/inference-engine/install_dependencies.sh

```bash
sudo apt install -y \
    build-essential \
    cmake \
    curl \
    wget \
    libssl-dev \
    ca-certificates \
    git \
    libboost-regex-dev \
    gcc-multilib \
    g++-multilib \
    libgtk2.0-dev \
    pkg-config \
    unzip \
    automake \
    libtool \
    autoconf \
    libcairo2-dev \
    libpango1.0-dev \
    libglib2.0-dev \
    libgtk2.0-dev \
    libswscale-dev \
    libavcodec-dev \
    libavformat-dev \
    libgstreamer1.0-0 \
    gstreamer1.0-plugins-base \
    libusb-1.0-0-dev \
    libopenblas-dev

# ubuntu 18.04
sudo apt install -y libpng-dev

# ubuntu 16.04
sudo apt install -y libpng12-dev

```

## 3. build

## 4. test

