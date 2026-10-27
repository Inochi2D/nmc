# Building NMC

## Building on Windows

TODO

## Building on Linux

```
# 0. Create directory for NMC and dependencies.
mkdir -p nmc
cd nmc

# 1. Clone dependencies and NMC.
git clone https://github.com/llvm/llvm-project.git
git clone https://github.com/Inochi2D/nmc.git

# 2. Build LLVM 24
cmake -S llvm-project/llvm -B llbuild/ -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=install/ -DLLVM_ENABLE_PROJECTS="clang;lld" -DLLVM_BUILD_TOOLS=ON -DLLVM_OPTIMIZED_TABLEGEN=ON -DLLVM_TARGETS_TO_BUILD="X86;AArch64;WebAssembly"
ninja -C llbuild/ -j 16 install

# 3. Build NMC
cmake -S nmc -B nmbuild/ -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=install/ -DLLVM_ROOT_DIR=install/ -DNMC_LINK_MANUALLY=ON
ninja -C nmbuild/ -j 16 install
```


## Building on macOS

```
# 0. Create directory for NMC and dependencies.
mkdir -p nmc
cd nmc

# NOTE: This is needed on macOS 27+ to avoid build errors.
export MACOSX_DEPLOYMENT_TARGET=$(uname -r)

# 1. Clone dependencies and NMC.
git clone https://github.com/facebook/zstd.git
git clone https://github.com/llvm/llvm-project.git
git clone https://github.com/Inochi2D/nmc.git

# 2. Build zstd
cmake -S zstd -B zsbuild -G Ninja -DCMAKE_OSX_ARCHITECTURES="x86_64;x86_64h;arm64" -DCMAKE_INSTALL_PREFIX=install/
ninja -C zsbuild/ -j 16 install

# 3. Build LLVM 24
cmake -S llvm-project/llvm -B llbuild/ -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=install/ -DLLVM_ENABLE_PROJECTS="clang;lld" -DLLVM_BUILD_TOOLS=ON -DLLVM_OPTIMIZED_TABLEGEN=ON -DLLVM_TARGETS_TO_BUILD="X86;AArch64;WebAssembly" -DLLVM_NO_INSTALL_NAME_DIR_FOR_BUILD_TREE=ON
ninja -C llbuild/ -j 16 install

# 4. Build NMC
cmake -S nmc -B nmbuild/ -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=install/ -DLLVM_ROOT_DIR=install/ -DNMC_LINK_MANUALLY=ON
ninja -C nmbuild/ -j 16 install
```
