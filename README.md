# ACAP OpenCV

Based on acap-native-sdk

https://hub.docker.com/r/axisecp/acap-native-sdk

## Package Added

- cmake
- opencv /opt

## cmake

```cmake
include_directories(/opt/app/include)
link_directories(/opt/app/lib)

add_executable(app main.cpp)
target_link_libraries(app opencv_core opencv_imgcodecs opencv_imgproc)
```

## DockerHub

https://hub.docker.com/r/bellx2/acap_opencv
