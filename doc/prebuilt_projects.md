# NDK

```
mkdir ndk-project && cd ndk-project
repo init -u git@github.com:riscv-android-src/manifest.git -b ndk-r20-riscv64
repo sync
./ndk/checkbuild.py
```

# CLANG/LLVM

```
mkdir clang-toolchain && cd clang-toolchain
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv-llvm-toolchain-android-10_dev
repo sync
python toolchain/llvm_android/build.py 
```

# RUST Toolchain

```
mkdir rust-toolchain && cd rust-toolchain
repo init -u https://android.googlesource.com/platform/manifest -b rust-toolchain
repo sync
./toolchain/android_rust/build.py
```

# Linux kernel

```
git clone https://android.googlesource.com/kernel/common -b android12-5.10-lts
cd common
make ARCH=riscv defconfig
make ARCH=riscv CROSS_COMPILE=riscv64-linux-android-
```
