# Setup Android 12 on RISC-V

To download the RISC-V Android source tree to your working directory:

```bash
mkdir ~/riscv-android-src && cd ~/riscv-android-src
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv64-android-12.0.0_dev
repo sync
cd prebuilts/rust/
git lfs pull
cd -
cd cts/
git lfs pull
cd -
rm external/angle/Android.bp
```

Currently ART is not fully functional yet, to make Zygote running on the andorid emulator, apply following patches before continuing.

```bash
riscv-android-src/art$ git diff
diff --git a/runtime/jit/jit.h b/runtime/jit/jit.h
index a6e484f563..234c2a0693 100644
--- a/runtime/jit/jit.h
+++ b/runtime/jit/jit.h
@@ -122,7 +122,7 @@ class JitOptions {
   }
 
   bool UseJitCompilation() const {
-    return use_jit_compilation_;
+    return false;
   }
 
   bool UseProfiledJitCompilation() const {
diff --git a/runtime/thread.h b/runtime/thread.h
index 7a408021c1..22be55c529 100644
--- a/runtime/thread.h
+++ b/runtime/thread.h
@@ -1277,7 +1277,7 @@ class Thread {
   }
 
   bool IsForceInterpreter() const {
-    return tls32_.force_interpreter_count != 0;
+    return true;
   }
 
   bool IncrementMakeVisiblyInitializedCounter() {
```

plus:
```bash
riscv-android-src/build/make$ git diff
diff --git a/core/board_config.mk b/core/board_config.mk
index 1b08f9a0b4..c8dc2d7fd8 100644
--- a/core/board_config.mk
+++ b/core/board_config.mk
@@ -149,7 +149,7 @@ _board_strip_readonly_list += $(_build_broken_var_list) \
 
 # Conditional to building on linux, as dex2oat currently does not work on darwin.
 ifeq ($(HOST_OS),linux)
-  WITH_DEXPREOPT := true
+  WITH_DEXPREOPT := false
 endif
 
 # ###############################################################
```

Build full emulator image with command:

```bash
source build/envsetup.sh
lunch sdk_phone64_riscv64
m -j
```

Run the RISC-V 64 AVD system image in the Android Emulator:

For non-graphical interface:
```bash
emulator -no-qt -show-kernel -noaudio -selinux permissive -qemu -smp 1 -m 3584M -bios kernel/prebuilts/5.10/riscv64/fw_jump.bin
```

For graphical interface:
```bash
emulator -verbose -no-boot-anim -show-kernel -noaudio -selinux permissive -qemu -smp 1 -m 3584M -bios kernel/prebuilts/5.10/riscv64/fw_jump.bin
```
Notice: Ubuntu 18.04 and 20.04 should work, but 22.04 may crash.

