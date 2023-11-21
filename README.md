# ACAP OpenCV

Based on acap-native-sdk

https://hub.docker.com/r/axisecp/acap-native-sdk

## Package Added

- cmake
- opencv /opt

## cmake

```cmake
set(OpenCV_DIR /opt/lib/)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIBRARY_DIRS})
target_link_libraries(app ${OpenCV_LIBS})

link_directories(/opt/app/lib)
```

## DockerHub

https://hub.docker.com/r/bellx2/acap_opencv
