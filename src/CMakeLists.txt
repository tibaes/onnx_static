cmake_minimum_required(VERSION 3.13)
project(onnxstatic CXX)

option(ONNXRUNTIME_INCLUDE_DIR "onnxruntime include dir")
include_directories("${ONNXRUNTIME_INCLUDE_DIR}" "${ONNXRUNTIME_INCLUDE_DIR}/onnxruntime/core/session")

option(ONNXRUNTIME_BUILD_DIR "onnxruntime build dir")
if (WIN32)
  file(GLOB_RECURSE ONNXRuntime_LIBS
    LIST_DIRECTORIES false
    ${ONNXRUNTIME_BUILD_DIR}/*.lib
  )
else()
  file(GLOB_RECURSE ONNXRuntime_LIBS
    LIST_DIRECTORIES false
    ${ONNXRUNTIME_BUILD_DIR}/*.a
  )
endif()
message("ONNXuntime_LIBS: " ${ONNXRuntime_LIBS})

add_executable(onnxio onnx_model_io.cpp)
target_link_libraries(onnxio PRIVATE ${ONNXRuntime_LIBS})
