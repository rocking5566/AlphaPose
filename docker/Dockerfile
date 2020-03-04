FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

# Install some dependencies
RUN apt-get update && \
    apt-get install -y libyaml-dev \
    software-properties-common \
    g++ \
    unzip \
    vim \
    build-essential \
    git \
    wget \
    cmake \
    libgtk2.0-dev \
    pkg-config \
    libcanberra-gtk-module \
    libavcodec-dev \
    libavformat-dev \
    libswscale-dev

RUN add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y \   
      python3.6 \
      python3.6-dev \
      python3-pip \
      python3.6-tk \
      python-pip

RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 1 && \
    rm /usr/bin/python && ln -s /usr/bin/python3.6 /usr/bin/python

RUN update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

RUN pip install --upgrade pip
RUN pip --no-cache-dir install --upgrade Cython python-dateutil
RUN pip --no-cache-dir install Pillow==5.3.0 \
  numpy==1.14.5 \
  scipy==1.1.0 \
  sklearn \
  scikit-image \
  pandas \
  matplotlib==2.2.2 \
  requests \
  pika \
  h5py

RUN pip --no-cache-dir install decorator \
    networkx \
    nose \
    python-gflags \
    pyyaml \
    six==1.12.0 \
    future \
    pathlib \
    tqdm==4.23.4 \
    sklearn \
    requests \
    easydict \
    munkres==1.0.12 \
    tensorboardx \
    terminaltables \
    visdom \
    opencv_python==3.4.2.16 \
    opencv_contrib_python==3.4.2.16 \
    Image==1.5.25 \
    pycocotools

RUN pip install --no-cache-dir torch==1.1.0 
RUN pip install --no-cache-dir torchvision==0.3.0 torchsummary
RUN pip install git+https://github.com/yanfengliu/cython_bbox.git

# ============= OpenCV =============
# OpenCV
# [Warning] - opencv-python does not include Video I/O (Can not open Webcam)
# Build opencv from source

ENV OPENCV_DIR /opt

RUN mkdir -p $OPENCV_DIR 

WORKDIR $OPENCV_DIR

ENV OPENCV_VERSION="4.1.1"
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
  && unzip ${OPENCV_VERSION}.zip \
  && mkdir opencv-${OPENCV_VERSION}/build \
  && cd opencv-${OPENCV_VERSION}/build \
  && cmake -DBUILD_TIFF=ON \
    -DBUILD_opencv_java=OFF \
    -DWITH_CUDA=OFF \
    -DWITH_IPP=ON \
    -DWITH_TBB=ON \
    -DWITH_EIGEN=ON \
    -DWITH_V4L=ON \
    -DBUILD_TESTS=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DCMAKE_BUILD_TYPE=RELEASE \
    -DCMAKE_INSTALL_PREFIX=$(python3.5 -c "import sys; print(sys.prefix)") \
    -DPYTHON_EXECUTABLE=$(which python3.5) \
    -DPYTHON_INCLUDE_DIR=$(python3.5 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
    -DPYTHON_PACKAGES_PATH=$(python3.5 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
    .. \
  && make -j"$(nproc)" && make install \
  && cd /opt \
  && rm ${OPENCV_VERSION}.zip \
  && rm -r opencv-${OPENCV_VERSION}

# ============= OpenCV End =============
ENV PYTHONPATH /workspace:$PYTHONPATH