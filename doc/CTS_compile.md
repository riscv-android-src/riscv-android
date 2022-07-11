# CTS compile

> you need to switch to the project directory
>

#### 1. cd  riscv-android-src 

> import build script environment
>

#### 2. source build/envsetup.sh

> select the platform you want to build, you can view and select through the lunch command
>

#### 3. lunch sdk_phone64_riscv64

> compile the cts module
>

#### 4. m cts -j16



### Error when Build the CTS: cannot find symbol

```javascript
"out/soong/.intermediates/cts/tests/MediaProviderTranscode/CtsMediaProviderTranscodeTests/android_common/javac/srcjars"
cts/tests/MediaProviderTranscode/src/android/mediaprovidertranscode/cts/TranscodeTestUtils.java:97: error: cannot find symbol
        return stageVideoFile(videoFile, R.raw.testVideo_HEVC_long);
                                              ^
  symbol:   variable testVideo_HEVC_long
  location: class raw
1 error
[ 96% 55786/58053] //cts/tests/tests/tools/processors/view_inspector:CtsViewInspectorAnnotationProc
Warning: Missing class androidx.annotation.experimental.Experimental$Level (referenced from: androidx.test.services.storage.ExperimentalTestStorage)
Missing class androidx.annotation.experimental.Experimental (referenced from: androidx.test.services.storage.ExperimentalTestStorage)
```

Solution: Download testVideo_HEVC_long.mp4 from the corresponding version on aosp and place it. The specific directory is located in cts/tests/MediaProviderTranscode/res/raw