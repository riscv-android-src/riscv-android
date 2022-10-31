# NDK

```
# Android 10
mkdir ndk-project && cd ndk-project
repo init -u git@github.com:riscv-android-src/manifest.git -b ndk-r20-riscv64
repo sync
./ndk/checkbuild.py

# Android 12
mkdir ndk-project && cd ndk-project
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv-ndk-release-r23
repo sync
./ndk/checkbuild.py
## windows
./ndk/checkbuild.py --system windows64

## Pack output
$cd out/linux
$tar zcvf android-ndk-r23b.linux.tar.gz android-ndk-r23b
$cd -
$cd out/windows64
$tar zcvf android-ndk-r23b.win64.tar.gz android-ndk-r23b
```

# CLANG/LLVM

```
# Android 10
mkdir clang-toolchain && cd clang-toolchain
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv-llvm-toolchain-android-10_dev
repo sync
python toolchain/llvm_android/build.py

# Android 12
mkdir clang-toolchain && cd clang-toolchain
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv-llvm-r416183_dev
repo sync
python toolchain/llvm_android/build.py --no-build lldb
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

# Emulator

```
repo init -u git@github.com:riscv-android-src/manifest.git -b emu-31.2.1.0-riscv64
repo sync
cd external/qemu/
./android/rebuild.sh
ls objs/distribution/
```
