# TensorMDv2

## 远程仓库
[tenosrmdv2](https://gitee.com/bismarrck/lammps)
- tensormd分支：主开发分支

## 编译
### 依赖库
- yaml-cpp
- cnpy++
- libzip

```bash
cmake .. -DENABLE_BZIP2=OFF  -DENABLE_LZMA=OFF -DENABLE_OPENSSL=OFF
make
make install DESTDIR=/install/dir/
```
- Boost >= 1.78

```bash
./bootstrap.sh --prefix=/install/dir/
./b2 install
```

### 编译命令

```bash
export CC=mpicc
export CXX=mpicxx

cmake ../cmake \
    -C ../cmake/presets/tensormd.cmake \
    -D CMAKE_BUILD_TYPE=Release \
    -D BUILD_MPI=yes \
    -D BUILD_OMP=yes \
    -D BUILD_GPU=yes \
    -D BUILD_CUDA=yes \
    -D CMAKE_PREFIX_PATH="/libzip/install/dir/;/boost/install/dir"

make -j16 VERBOSE=1
```