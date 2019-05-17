# Linux setup

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

```bash
cd WORK_DIR
mkdir build
cd build

cmake -DCMAKE_BUILD_TYPE=Release ..
make -j8
```

## 4. test

### a. download model

download SSD caffe model from this link

https://github.com/weiliu89/caffe/tree/ssd
```
deploy.prototxt
VGG_VOC0712_SSD_300x300_iter_120000.caffemodel
```

### b. convert model

```bash
cd dldt-2019_R1.0.1/model-optimizer
python3 ./mo_caffe.py --input_model ~/tmp/VGG_VOC0712_SSD_300x300_iter_120000.caffemodel --input_proto ~/tmp/deploy.prototxt  --output_dir ./
```

### c. run sample application

**inference on CPU**

```bash
./object_detection_sample_ssd -i ~/tmp/cat.jpg -m ~/tmp/VGG_VOC0712_SSD_300x300_iter_120000.xml
```

***inference on GPU**

to use GPU, need install intel opencl compute runtime driver. refer to

https://github.com/mintaka33/vaapi-opencl-interop/blob/master/README.md

```bash
./object_detection_sample_ssd -i ~/tmp/cat.jpg -m ~/tmp/VGG_VOC0712_SSD_300x300_iter_120000.xml -d GPU
```
