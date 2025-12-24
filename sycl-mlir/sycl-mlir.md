# sycl-mlir

## 远程仓库
[sycl-mlir](https://github.com/0x2d/sycl-mlir)
- sycl-mlir分支：同步上游更新
- main-dev分支：主开发分支

## 编译
官方文档： [DPC++ doc](https://github.com/intel/llvm/blob/sycl-mlir/sycl/doc/GetStartedGuide.md) [SYCL-MLIR doc](https://github.com/intel/llvm/blob/sycl-mlir/mlir-sycl/doc/developer/ContributeToSYCLMLIR.md)

sycl-mlir需要联网下载依赖库，130服务器可以在.gitconfig中加入（需要使用root账号将ssh公钥加入repo账号的authorized_keys中）：
```bash
[url "repo@10.208.130.147:github.com/"]
  insteadOf = https://github.com/
```
其他平台安装可以仿照这个方式，通过上传git裸仓库提供离线依赖库；或通过ssh反向代理，从服务器访问外部网络。

由于OpenCL-Headers最新Commit与sycl-mlir不兼容，因此需要手动选择旧版本，替换OpenCL-Headers.git/refs/heads/main内容为：
```bash
8275634cf9ec31b6484c2e6be756237cb583999d
```

### hip编译

设置前端机环境
```bash
# set modules
module unload compiler/devtoolset/7.3.1
module unload mpi/hpcx/2.11.0/gcc-7.3.1
module unload compiler/rocm/dtk/22.10.1
module load compiler/rocm/dtk/25.04.1
# set python
conda activate oy_dev
# set gcc
source ~/Tools/setgcc.sh
```

编译命令
```bash
# sycl-mlir
python /public/home/liuying/sycl-mlir/buildbot/configure.py -o /public/home/liuying/sycl-mlir/build --build-compiler-c /public/home/liuying/Tools/gcc/11.2.0/bin/gcc --build-compiler-cpp /public/home/liuying/Tools/gcc/11.2.0/bin/g++
python /public/home/liuying/sycl-mlir/buildbot/compile.py -o /public/home/liuying/sycl-mlir/build -j 32

# llvm-dcu-offline-22-master
python /public/home/liuying/llvm-dcu-offline-22-master/buildbot/configure.py --hip --cmake-opt=-DSYCL_BUILD_PI_HIP_INCLUDE_DIR=/public/software/compiler/dtk/dtk-25.04.1/hip/include --cmake-opt=-DSYCL_BUILD_PI_HIP_HSA_INCLUDE_DIR=/public/software/compiler/dtk/dtk-25.04.1/hsa/include --cmake-opt=-DSYCL_BUILD_PI_HIP_AMD_LIBRARY=/public/software/compiler/dtk/dtk-25.04.1/lib/libamdhip64.so
python /public/home/liuying/llvm-dcu-offline-22-master/buildbot/compile.py  -j 32
```


## 设置环境变量
```bash
# unset other oneAPI and LLVM environment variables
export PATH=/path/to/install/bin:$PATH
export LD_LIBRARY_PATH=/path/to/install/lib:$LD_LIBRARY_PATH
# 130服务器需要额外提供tbb库
export LD_LIBRARY_PATH=/opt/intel/oneapi/tbb/2022.0/lib:$LD_LIBRARY_PATH  
```

## 测试
```bash
cd build
ninja check-mlir-sycl 
ninja check-cgeist
ninja check polygeist-opt
```
测试结果参见：[测试结果](./test.md)

## 重要的调试环境变量
```bash
export SYCL_PI_TRACE=-1
```

## 运行
```
# Example
clang++ -fsycl -fsycl-targets=spir64-unknown-unknown-syclmlir simple-sycl-app.cpp
# Options
-fsycl-raise-host # Raise SYCL host code to MLIR after compiling to LLVM IR
-fsycl-device-only # Only generate device code
-emit-mlir # Dump device MLIR
```
其他平台编译命令参见：[sw编译命令](./sw-compile-commands.md)，[hip编译命令](./hip-compile-commands.md)