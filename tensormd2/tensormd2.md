# TensorMD2

## Remote
[tenosrmd2](https://gitee.com/bismarrck/lammps) (Gitee)
[tenosrmd2](https://github.com/TensorMD/lammps-tensormd/tree/tensormd2) (GitHub)

## Compile
### Dependence
- MKL / BLAS
- yaml-cpp
- cnpy++
- libzip

```bash
cmake .. -DENABLE_BZIP2=OFF  -DENABLE_LZMA=OFF -DENABLE_OPENSSL=OFF -DENABLE_ZSTD=OFF
make
make install DESTDIR=/install/dir/
```
- Boost >= 1.78

```bash
./bootstrap.sh --prefix=/install/dir/
./b2 install
```

### Environment Variables on CNIS
```bash
# orise
module unload compiler/rocm/dtk/22.10.1
module load compiler/rocm/dtk/25.04.3
module load compiler/cmake/3.24.1
module load mathlib/openblas/gnu/0.3.7
module load compiler/llvm-openmp/openmp
module unload mpi/hpcx/2.11.0/gcc-7.3.1
module unload compiler/devtoolset/7.3.1
module load compiler/devtoolset/9.3.1
module load mpi/hpcx/2.13.1/gcc-9.3.1-wangxh

# cnis
module purge
module use /public/software/modules/
module load compiler/dtk/25.04.2
module load compiler/cmake/3.25.3
module load mathlib/OpenBLAS/0.3.21
module load compiler/gcc/9.3.0
module load mpi/openmpi/mlnx/5.0.0
```

### Compile Commands

```bash
export CC=mpicc
export CXX=mpicxx

# cuda
cmake ../cmake \
    -C ../cmake/presets/tensormd.cmake \
    -D CMAKE_BUILD_TYPE=Release \
    -D BUILD_MPI=yes \
    -D BUILD_OMP=yes \
    -D BUILD_GPU=yes \
    -D BUILD_CUDA=yes \
    -D YAMLCPP_REPO=/yaml-cpp-0.8.0/dir \
    -D CNPYPP_REPO=/cnpypp-master/dir \
    -D CMAKE_PREFIX_PATH="/libzip/install/dir/;/boost/install/dir"

# hip
export amd_comgr_DIR="/path/to/rocm/lib64/cmake/amd_comgr"
cmake ../cmake \
    -C ../cmake/presets/tensormd.cmake \
    -D CMAKE_BUILD_TYPE=Release \
    -D BUILD_MPI=yes \
    -D BUILD_OMP=yes \
    -D BUILD_GPU=yes \
    -D BUILD_HIP=yes \
    -D YAMLCPP_REPO=/yaml-cpp-0.8.0/dir \
    -D CNPYPP_REPO=/cnpypp-master/dir \
    -D CMAKE_PREFIX_PATH="/libzip/install/dir/;/boost/install/dir"

make -j16 VERBOSE=1
```