# onnx_static
How to hack ONNXRuntime for creating static libraries for usage on C++ projects

First be sure to clone all submodules on this repository in order to fetch `ONNXRuntime`.
```
git submodule update --init --recursive
```

Then build `ONNXRuntime` in static mode. Here we use turn on XNN backend and some extra optimizations. 
```
mkdir build_onnx
cd build_onnx
cmake -G Ninja ..\onnxruntime\cmake\ -DCMAKE_BUILD_TYPE="Release" -Donnxruntime_USE_AVX=ON -Donnxruntime_USE_AVX2=ON -Donnxruntime_USE_AVX512=ON -Donnxruntime_USE_XNNPACK=ON
ninja
```

It's a good practice to test out unit tests by running `onnxruntime_test_all` executable.

After building `ONNXRuntime`, get back to this project root folder. Then let's build our model IO tool as a demo of how to build any executable requiring ONNX in a static way, i.e. without relying on dynamic libraries on runtime.
```
mkdir build
cd build
cmake -G Ninja ..\src -DCMAKE_BUILD_TYPE="Release" -DONNXRUNTIME_INCLUDE_DIR="..\onnxruntime\include"  -DONNXRUNTIME_BUILD_DIR="..\build_onnx" 
ninja
```

Now some executable should have been created, like `build\onnxio.exe`. Test it out by passing the path of some `.onnx` model as parameter. It should output the model IOs description.

License note:
The file `src/onnx_model_io.cpp` is a copy from [Microsoft repository](https://github.com/microsoft/onnxruntime-inference-examples/blob/main/c_cxx/model-explorer/model-explorer.cpp) without any modifications. It is here for mere convenience.