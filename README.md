# ACAP OpenCV

Based on acap3-sdk

https://hub.docker.com/r/axisecp/acap-sdk

## Package Added

- cmake
- opencv /opt/opencv/
- opencv_3rd  /opt/opencv/share/lib

## cmake

```cmake
set(OpenCV_DIR /opt/opencv/share/OpenCV)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIBRARY_DIRS})
target_link_libraries(app ${OpenCV_LIBS}

link_directories(/opt/opencv/share/lib)
```

## DockerHub

https://hub.docker.com/r/bellx2/acap_opencv
