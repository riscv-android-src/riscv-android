# Setup Android 12 on RISC-V

To download the RISC-V Android source tree to your working directory:

```bash
mkdir ~/riscv-android-src && cd ~/riscv-android-src
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv64-android-12.0.0_dev
repo sync
cd prebuilts/rust/
git lfs pull
cd -
cd  prebuilts/clang/host/linux-x86/
git lfs pull
cd -
rm external/angle/Android.bp
```

Before build, manually update `~/riscv-android-src/device/generic/goldfish/fstab.ranchu.riscv.ex` as below diff shows:

```
diff --git a/fstab.ranchu.riscv.ex b/fstab.ranchu.riscv.ex
index 69c180e9..dff1896a 100644
--- a/fstab.ranchu.riscv.ex
+++ b/fstab.ranchu.riscv.ex
@@ -6,6 +6,6 @@ system   /system     ext4    ro,barrier=1     wait,logical,first_stage_mount
 vendor   /vendor     ext4    ro,barrier=1     wait,logical,first_stage_mount
 product  /product    ext4    ro,barrier=1     wait,logical,first_stage_mount
 system_ext  /system_ext  ext4   ro,barrier=1   wait,logical,first_stage_mount
-/dev/block/vdc   /data     ext4      noatime,nosuid,nodev,nomblk_io_submit,errors=panic   wait,check,quota,fileencryption=aes-256-xts:aes-256-cts,reservedsize=128M,fsverity,keydirectory=/metadata/vold/metadata_encryption,latemount
+/dev/block/vdc  /data    ext4      noatime,nosuid,nodev,nomblk_io_submit,errors=panic   wait,check,quota,latemount
 /dev/block/platform/20000c00.virtio_mmio/by-name/metadata    /metadata    ext4    noatime,nosuid,nodev    wait,formattable,first_stage_mount
 /devices/*/block/vdf  auto  auto      defaults voldmanaged=sdcard:auto,encryptable=userdata
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

