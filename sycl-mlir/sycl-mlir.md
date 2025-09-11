# sycl-mlir

## 远程仓库
[sycl-mlir](https://github.com/0x2d/sycl-mlir)

sycl-mlir分支：同步上游更新

main-dev分支：主开发分支

## 编译
[DPC++ doc](https://intel.github.io/llvm/GetStartedGuide.html)

[SYCL-MLIR doc](https://github.com/intel/llvm/blob/sycl-mlir/mlir-sycl/doc/developer/ContributeToSYCLMLIR.md)

由于最新版的ocl header与sycl-mlir不兼容，因此需要手动选择旧版本依赖。130服务器可以在.gitconfig中加入：

```bash
[url "repo@10.208.130.147:github.com/"]
  insteadOf = https://github.com/
```

（需要使用root账号将ssh公钥加入repo账号的authorized_keys中）
	
## 设置环境变量
```bash
(unset other oneAPI and LLVM environment variables）
export PATH=/path/to/install/bin:$PATH
export LD_LIBRARY_PATH=/path/to/install/lib:/opt/intel/oneapi/tbb/2022.0/lib:$LD_LIBRARY_PATH  
```


## 测试
```bash
cd build
ninja check-mlir-sycl 
ninja check-cgeist
ninja check polygeist-opt
```
测试结果参见：[测试结果](./test.md)

## 运行
```
clang++ -fsycl -fsycl-targets=spir64-unknown-unknown-syclmlir -fsycl-raise-host simple-sycl-app.cpp -o simple-sycl-app
# Raise SYCL host code to MLIR after compiling to LLVM IR
clang++ -fsycl -fsycl-targets=spir64-unknown-unknown-syclmlir -fsycl-raise-host simple-sycl-app.cpp -o simple-sycl-app
# Dump device MLIR
clang++ -S -fsycl -emit-mlir -fsycl-device-only simple-sycl-app.cpp -o simple-sycl-app.mlir
```
具体编译命令参见：[编译命令](./compile-commands.md)