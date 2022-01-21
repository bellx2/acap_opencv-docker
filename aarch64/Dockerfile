ARG ARCH=aarch64
ARG VERSION=3.4.2
ARG UBUNTU_VERSION=20.04
ARG REPO=axisecp
ARG SDK=acap-sdk

FROM ${REPO}/${SDK}:${VERSION}-${ARCH}-ubuntu${UBUNTU_VERSION} as builder

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -yf --no-install-recommends \
    ca-certificates \
    cmake && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# OpenCV
ARG OPENCV_VERSION=3.4.14
WORKDIR /workspace
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN curl -fsSL https://github.com/opencv/opencv/archive/$OPENCV_VERSION.tar.gz | tar -xz
WORKDIR /workspace/opencv-$OPENCV_VERSION/build
ADD "https://www.random.org/cgi-bin/randbyte?nbytes=10&format=h" /dev/null
RUN cmake -D CMAKE_TOOLCHAIN_FILE=../platforms/linux/aarch64-gnu.toolchain.cmake \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/opt/opencv \
    -D WITH_OPENEXR=OFF \
    -D WITH_GTK=OFF \
    -D WITH_V4L=OFF \
    -D WITH_FFMPEG=OFF \
    -D WITH_GSTREAMER=OFF \
    -D WITH_GSTREAMER_0_10=OFF \
    -D BUILD_LIST=core,highgui,imgcodecs,imgproc,videoio \
    -D BUILD_EXAMPLES=OFF \
    -D BUILD_OPENCV_APPS=OFF \
    -D BUILD_DOCS=OFF \
    -D BUILD_TESTS=OFF \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_JPEG=ON \
    -D BUILD_PNG=ON \
    -D BUILD_TIFF=OFF \
    -D BUILD_WEBP=OFF \
    -D WITH_JASPER=OFF \
    -D BUILD_PROTOBUF=OFF \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    .. && \
    make -j 4 install
RUN cp -r 3rdparty/lib/ /opt/opencv/share/

FROM ${REPO}/${SDK}:${VERSION}-${ARCH}-ubuntu${UBUNTU_VERSION}
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -yf --no-install-recommends \
    ca-certificates \
    cmake && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
COPY --from=builder /opt/opencv /opt/opencv