ARG ARCH=armv7hf
ARG VERSION=1.3
ARG UBUNTU_VERSION=22.04
ARG REPO=axisecp
ARG SDK=acap-native-sdk

FROM ${REPO}/${SDK}:${VERSION}-${ARCH}-ubuntu${UBUNTU_VERSION} as builder

# Set general arguments
ARG SDK_LIB_PATH_BASE=/opt/axis/acapsdk/sysroots/${ARCH}/usr
ARG BUILD_DIR=/opt/build

WORKDIR ${BUILD_DIR}
RUN apt-get update && apt-get install --no-install-recommends -y cmake && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# OpenCV
ARG OPENCV_VERSION=4.5.1
ARG OPENCV_DIR=${BUILD_DIR}/opencv
ARG OPENCV_SRC_DIR=${OPENCV_DIR}/opencv-${OPENCV_VERSION}
ARG OPENCV_BUILD_DIR=${OPENCV_DIR}/build

WORKDIR ${OPENCV_DIR}
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN curl -fsSL https://github.com/opencv/opencv/archive/$OPENCV_VERSION.tar.gz | tar -xz

WORKDIR ${OPENCV_BUILD_DIR}
ENV COMMON_CMAKE_FLAGS="-S $OPENCV_SRC_DIR \
        -B $OPENCV_BUILD_DIR \
        -D CMAKE_INSTALL_PREFIX=/opt \
        -D CMAKE_BUILD_TYPE=RELEASE \
        -D WITH_OPENEXR=OFF \
        -D WITH_GTK=OFF \
        -D WITH_V4L=OFF \
        -D WITH_FFMPEG=OFF \
        -D WITH_GSTREAMER=OFF \
        -D WITH_GSTREAMER_0_10=OFF \
        -D BUILD_LIST=core,imgproc,video,imgcodecs \
        -D BUILD_EXAMPLES=OFF \
        -D BUILD_OPENCV_APPS=OFF \
        -D BUILD_DOCS=OFF \
        -D BUILD_JPEG=ON \
        -D BUILD_PNG=OFF \
        -D WITH_JASPER=OFF \
        -D BUILD_PROTOBUF=OFF \
        -D OPENCV_GENERATE_PKGCONFIG=ON "

RUN . /opt/axis/acapsdk/environment-setup* && \
    # Configure build with CMake
    cmake \
    -D CMAKE_CXX_COMPILER=${CXX%-g++*}-g++ \
    -D CMAKE_CXX_FLAGS="${CXX#*-g++}" \
    -D CMAKE_C_COMPILER=${CC%-gcc*}-gcc \
    -D CMAKE_C_FLAGS="${CC#*-gcc}" \
    -D CMAKE_TOOLCHAIN_FILE=${OPENCV_SRC_DIR}/platforms/linux/arm-gnueabi.toolchain.cmake \
    -D CPU_BASELINE=NEON,VFPV3 \
    -D ENABLE_NEON=ON \
    -D ENABLE_VFPV3=ON \
    $COMMON_CMAKE_FLAGS && \
    # Build and install OpenCV
    make -j "$(nproc)" install ;

FROM ${REPO}/${SDK}:${VERSION}-${ARCH}-ubuntu${UBUNTU_VERSION}
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -yf --no-install-recommends \
    ca-certificates \
    cmake && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
WORKDIR /opt/app/lib
COPY --from=builder /opt/lib/* .
WORKDIR /opt/app/include
COPY --from=builder /opt/include/opencv4 .
