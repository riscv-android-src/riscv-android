# Common faults and treatment methods

Here is a collection of common faults and the corresponding treatment methods.

PS: the faults marked as "*" are due to old version/compatibility problem， can solved by update new version source code.

# Missing dependency

The Android system build depend on some commonly used tools like repo, git, curl, unzip and etc.

```plain
bash: repo: command not found...
```

If you find any missing dependency you can follow the instructions in the link below to install them：

```plain
https://source.android.com/setup/build/initializing
```

# Source sync fail

If you get some error while synchronize source code with repo, you can check the link below for instructions:

```
https://source.android.com/setup/build/downloading
```

# Build error

If the build is failed with following Error messages:

```
prebuilts/clang-tools/linux-x86/bin/versioner: error while loading shared libraries: libclang_cxx.so.11git: cannot open shared object file: No such file or directory
```

You can export a environment variable to walkaround this error:

```
export LD_LIBRARY_PATH="~/riscv-android-src/prebuilts/clang/host/linux-x86/clang-dev/lib64/"
```

# System boot fail

If the Android Runtime fail to load libcompiler_rt.so while initialization, you may need to manually copy this file to system directory.

```
cp ./out/target/product/generic_riscv64/system/lib64/vndk-sp-29/libcompiler_rt.so ./out/target/product/generic_riscv64/system/lib64/libcompiler_rt.so
```

