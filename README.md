# RISC-V Android 

This is the RISC-V Android collaborative source code repo. It contains all the modified AOSP(Android open source project) repositories with RISC-V support, while other non-architecture related repositories can be directly downloaded from the official AOSP repo.

The main purpose of this repo is to provide a codebase for the developers, continuously improve the RISC-V architecture support with the collaboration of community and upstream these support to the official AOSP repo.

## Getting started 

Before downloading RISC-V Android source code please check you work environment, it is recommended to install a Linux system(Ubuntu 18.04 is preferred) with at least 250G disk space, 16 GB+ of RAM and more than 6 CPU cores. 

To download the RISC-V Android source tree to your working directory:

```bash
mkdir ~/riscv-android-src && cd mkdir ~/riscv-android-src
repo init -u git@github.com:riscv-android-src/manifest.git -b riscv64-android-10.0.0_dev
repo sync
```

If you are not able to access the official AOSP repo, you may replace the download url with a mirror site:

```
export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'
# Modify the .repo/manifests/default.xml
-  <!--- use tsinghua android repo as code base
   <remote  name="tsinghua"
            fetch="https://mirrors.tuna.tsinghua.edu.cn/git/AOSP"
            review="https://android-review.googlesource.com/" />
   <default revision="refs/tags/android-10.0.0_r11"
            remote="tsinghua"
            sync-j="4" />
-  -->

   <default revision="refs/tags/android-10.0.0_r11"
-           remote="aosp"
+           remote="tsinghua"
            sync-j="4" />
```

To see the full list of available commands, run:

```
hmm
```

Build full emulator image with command:

```
source build/envsetup.sh
lunch aosp_riscv64-eng
m -j
```

Run the RISC-V 64 AVD system image in the Android Emulator:

```bash
emulator -selinux permissive -qemu -m 3800M -bios prebuilts/qemu-kernel/riscv64/ranchu/fw_jump.bin 
The options before -qemu are handled by emulator:
-debug                 enable/disable debug messages
-show-kernel           display kernel messages
-no-snapshot           perform a full boot and do not auto-save
-ports 7099,7100       TCP ports used for the console and adb bridge
The options after -qemu are handled by qemu:
-smp 2                 use multi cpu cores
-gdb tcp::2345         wait for gdb connection on tcp port 2345
Other options can refer to:
emulator -help or emulator -qemu -help
```

Usage of other branch may refer to:  [Branches and configurations](https://github.com/riscv-android-src/riscv-android/blob/main/doc/Branches_and_configurations.md)

If you want to rebuild the prebuild projects，please check：[Prebuilt projects](https://github.com/riscv-android-src/riscv-android/blob/main/doc/prebuilt_projects.md)

If you have encountered any problem with the environment setup，please check：[Common faults and treatment methods](https://github.com/riscv-android-src/riscv-android/blob/main/doc/common_faults_and_treatment_methods.md)

## Contribute

The Github projects and pull requests are used to manage development and code review.

If you are looking for some project to start with:

   1. Check the [TODO list](https://github.com/riscv-android-src/riscv-android/blob/main/doc/TODO_list.md) to find if there is any submodule you are interested in.
   2. If the submodule doesn't have an owner，you can volunteer to maintain this submodule.
   3. If the submodule does have an owner,  you can check the projects card board of this submodule(For example, [issues for RISC-V Android](https://github.com/orgs/riscv-android-src/projects/1 ) for general issues).

To contribute patches:

1. Fork the modified repository into a personal account.

2. Create a development branch named suitably for your work.

3. Replace the project remote and revision with your own repository:

   ```
   - <project path="bionic" name="platform-bionic" groups="pdk" remote="riscv-android" revision="riscv64-android-10.0.0_dev" />
   + <project path="bionic" name="platform-bionic" groups="pdk" remote="your_personal_remote" revision="riscv64-android-10.0.0_dev_xxx_support" />
   ```

4. All the new work should base on the corresponding development branch. 

5. Create commits making incremental, distinct, logically complete changes with appropriate commit messages.

6. It is better to include unit test result while add a new submodule support.

7. Push the development branch to your personal repository fork on Github.

8. Create a Github Pull Request targeting the the corresponding development branch. Associate the Pull Request with issue if it is an bug fix.

## Recent Updates

2021/11/03		Add RISC-V support for Android rust toolchain

2021/10/20		Upload the riscv64-android-10.0.0_dev branch to this repo

## Further reading

### Articles

- [How Alibaba is Porting RISC-V to the Android OS](https://riscv.org/blog/2021/11/how-alibaba-is-porting-risc-v-to-the-android-os-guoyin-chen-alibaba/)

### Slides

- [Porting Android onto RISC-V ](https://chipsalliance.org/wp-content/uploads/sites/83/2021/10/porting-android-chips_alliance-slides-v1.2-Han-Mao.pdf)

### Videos

- [Porting Android onto RISC-V(4：20~ 20：20)](https://www.youtube.com/watch?v=auXZdPwYs10&amp;amp;t=218)

## Related links

RISC-V Android Source main page: https://github.com/riscv-android-src

RISC-V Android SIG: https://lists.riscv.org/g/sig-android

RISC-V Android SIG administrative repository: https://github.com/riscv-admin/android

Official AOSP repo: https://android.googlesource.com/

AOSP website: https://source.android.com/

RISC-V international tech wiki: https://wiki.riscv.org/display/TECH

## License

We do not require any formal copyright assignment or contributor license agreement.  Any contributions intentionally sent upstream are presumed to be offered under terms of the OSI-approved Apache License 2.0. See LICENSE file for details.

## About Us

This repo is maintained by Android SIG(special interest group) under RISC-V international software HC.

The Android SIG([CHARTER](https://github.com/riscv-admin/android/blob/main/CHARTER.md)) is aimed to coordinate efforts of developers working on the RISC-V architecture support of Android and help upstream the RISC-V support to official AOSP repo.

Please hop in and join us to the discussions. You can subscribe the mailing lists [here](https://lists.riscv.org/g/sig-android). 



