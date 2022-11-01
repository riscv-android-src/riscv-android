# Build Gradle for RISC-V
1. Download source code of android studio
```
mkdir studio-src
cd studio-src
repo init -u https://android.googlesource.com/platform/manifest -b studio-main
repo sync -c -j4 -q
```

2. Apply patch for risc-v support
```
git fetch https://android.googlesource.com/platform/tools/base refs/changes/30/2280530/1 && git cherry-pick FETCH_HEAD
```

3. Remove dependent modules under tools/base
```
diff --git a/bazel/toplevel.WORKSPACE b/bazel/toplevel.WORKSPACE
index c723bba84b..e334c4a0f3 100644
--- a/bazel/toplevel.WORKSPACE
+++ b/bazel/toplevel.WORKSPACE
@@ -22,17 +22,17 @@ new_local_repository(
     build_file = "prebuilts/studio/jdk/BUILD.studio_jdk",
 )
 
-local_repository(
-    name = "blaze",
-    path = "tools/vendor/google3/blaze",
-    repo_mapping = {
-     "@local_jdk": "@studio_jdk",
-    },
-)
-
-load("@blaze//:binds.bzl", "blaze_binds")
-
-blaze_binds()
+#local_repository(
+#    name = "blaze",
+#    path = "tools/vendor/google3/blaze",
+#    repo_mapping = {
+#     "@local_jdk": "@studio_jdk",
+#    },
+#)
+#
+#load("@blaze//:binds.bzl", "blaze_binds")
+
+#blaze_binds()  
diff --git a/bazel/repositories.bzl b/bazel/repositories.bzl
index b2cb0273c7..f119e61cba 100644
--- a/bazel/repositories.bzl
+++ b/bazel/repositories.bzl
@@ -86,10 +86,6 @@ _git = [
         "name": "grpc_repo",
         "path": "external/grpc-grpc",
     },
-    {
-        "name": "androidndk",
-        "api_level": 21,
-    },
     {
         "name": "gflags_repo",
         "path": "external/gflags",
```

4. Change gradle build version
```
vi buildSrc/base/version.properties

# Release version definition

# This file is read by gradle build scripts, but also packaged with builder and
# builder-model as a resource, for Version classes to read.

baseVersion = 30.3.0
buildVersion = 7.3.0 #should be same as the version being used in the studio
cmdlineToolsVersion = 7.0
```

5. build gradle plugin
```
cd studio_src/tools
./gradlew :publishAndroidGradleLocal
```
# Build Application for RISC-V

1. Creat an new project in Android studio
   - File->New->New Project
   - Chose an templates->Next
   - Change settings for the project
2. Add JNI call example(Can be different when you are trying to write your own application)
   - Project navigator> [right click] > Add C++ to module
   - Add JNI example code
```
#include <jni.h>
#include <string>

extern "C" JNIEXPORT jstring JNICALL
Java_com_example_myapplication_FirstFragment_stringFromJNI(
        JNIEnv* env,
        jobject /* this */) {
    std::string hello = "Hello from C++ LCY";
    return env->NewStringUTF(hello.c_str());
}
```

   - Add JNI call in Java
```
public class MainActivity extends AppCompatActivity {

+    static {
+        System.loadLibrary("myapplication");
+    }

    private ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        // Example of a call to a native method
+        TextView tv = binding.sampleText;
+        tv.setText(stringFromJNI());
    }

+    public native String stringFromJNI();
}
```

3. Download gradle plugin from or build plugin with steps from section 1
   - Extract tarball to an directory
   - Add gradle path to build.gradle
```
buildscript {
    repositories {
        maven { url "/Users/maohan/Downloads/repo" }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:7.3.0'
    }
}
plugins {
    id 'com.android.application' version '7.3.0' apply false
    id 'com.android.library' version '7.3.0' apply false
}
```

   - Update all of the versions for corresponding gradle plugin
4. Get ndk for risc-v
   - build from source
```
# Download
mkdir ~/riscv-ndk-12-src && cd ~/riscv-ndk-12-src
repo init -u http://aligerrit.alibaba-inc.com/a/thead-aosp/platform/manifest -b riscv-ndk-release-r23
repo sync

# build
cd ~/riscv-ndk-12-src && ./ndk/checkbuild.py
```

   - Set ndk.dir in local.properties to ndk install dir
```
sdk.dir=/Users/maohan/Library/Android/sdk
+ndk.dir=/Users/maohan/Library/Android/sdk/ndk/32.0.0
```

5. Make the project
   - Build > Make projcet
```
If you meet this link error
ld: error: undefined symbol: operator delete(void*)
>>> referenced by new:334 (/Users/maohan/Library/Android/sdk/ndk/23.1.7779620/toolchains/llvm/prebuilt/darwin-x86_64/sysroot/usr/include/c++/v1/new:334)
>>>               CMakeFiles/myapplication.dir/myapplication.cpp.o:(Java_com_example_myapplication_FirstFragment_stringFromJNI)
>>> referenced by new:334 (/Users/maohan/Library/Android/sdk/ndk/23.1.7779620/toolchains/llvm/prebuilt/darwin-x86_64/sysroot/usr/include/c++/v1/new:334)
>>>               CMakeFiles/myapplication.dir/myapplication.cpp.o:(Java_com_example_myapplication_FirstFragment_stringFromJNI)

ld: error: undefined symbol: __gxx_personality_v0
>>> referenced by myapplication.cpp
>>>               CMakeFiles/myapplication.dir/myapplication.cpp.o:(DW.ref.__gxx_personality_v0)
clang-12: error: linker command failed with exit code 1 (use -v to see invocation)

Please add following line in CMakeLists.txt
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -lc++")
```
