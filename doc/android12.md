# Setup Android 12 on RISC-V

To download the RISC-V Android source tree to your working directory:

```bash
mkdir ~/riscv-android-src && cd mkdir ~/riscv-android-src
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv64-android-12.0.0_dev
repo sync
cd prebuilts/rust/
git lfs pull
cd -
cd  prebuilts/clang/host/linux-x86/
git lfs pull
touch packages/modules/ArtPrebuilt/com.android.art-riscv64.apex
```

Build full emulator image with command:

```
source build/envsetup.sh
lunch sdk_phone64_riscv64
m -j
```

Run the RISC-V 64 AVD system image in the Android Emulator:

```bash
emulator -no-qt -show-kernel -noaudio -selinux permissive -qemu -smp 1 -m 3584M -bios kernel/prebuilts/5.10/riscv64/fw_jump.bin
```

