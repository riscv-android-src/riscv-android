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
mkdir andorid-kernel
cd andorid-kernel
repo init -u git@github.com:riscv-android-src/kernel-manifest.git -b riscv64-android12-5.10
repo sync
```

Currently, we have not supported building with clang, so please install riscv gcc on your building machine and add path of toolchain to env var PATH before continue.
Besides, you may have to update `andorid-kernel/common/build.config.riscv64` to set "CROSS_COMPILE" to your own triplet string. 

Then you can continue with:
```
BUILD_CONFIG=common/build.config.gki.riscv64 ./build/build.sh -j $(nproc)
BUILD_CONFIG=common-modules/virtual-device/build.config.virtual_device.riscv64 build/build.sh -j $(nproc)
```

Now you can find kernel image(Image) and kernel modules(.ko) in `andorid-kernel/out/android12-5.10/dist`.


# Emulator

```
repo init -u git@github.com:riscv-android-src/manifest.git -b emu-31.2.1.0-riscv64
repo sync
cd external/qemu/
./android/rebuild.sh
ls objs/distribution/
```
