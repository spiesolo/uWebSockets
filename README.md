<div align="center"><img src="misc/images/logo.png"/></div>

This is fork of `µWS` project, with modifications to support node, node-gyp, node-pre-gyp and cross compile with Android NDK for Android device.

# Cross-compile for Android

## Prerequisite

Cross-compiling nodejs, and node modules (should have support to node-pre-gyp) requires standalone Android NDK to be installed firstly. Android NDK can be downloaded at [Android developer]( https://developer.android.com/ndk/downloads/index.html) or [China Android developer](https://developer.android.google.cn/ndk/downloads/index.html) website for chinese mainland user.

Android NDK provides a script, at build/tools/make-standalong-toolchain.sh, that can be used with to generate the standalone toolchain. Below is an example used for standalone toolchain generation for Android Jellybean device.

```
# /path/to/android-ndk/build/tools/make-standalone-toolchain.sh \
    --toolchain=arm-linux-androideabi-4.6 \
    --arch=arm \
    --install-dir=/path/to/standalone/toolchain \
    --platform=android-17
```

Change \-\-toolchain & \-\-platform options to fit your Android version.

## Cross-toolchain setup

```
# export CC=/path/to/standalone/toolchain/bin/arm-linux-androideabi-gcc
# export AR=/path/to/standalone/toolchain/bin/arm-linux-androideabi-ar
# export CXX=/path/to/standalone/toolchain/bin/arm-linux-androideabi-g++
# export LINK=/path/to/standalone/toolchain/bin/arm-linux-androideabi-g++
```

The node-gyp will use these settings to compile and link the target.

## Install dependency packages

Just simply do __npm install__ in uWebSockets to install the dependency packages. node-pre-gyp is needed for the cross compiling.

## Configure

```
# ./node_modules/node-pre-gyp/bin/node-pre-gyp --target_arch=arm --target_platform=android --target=6.10.3 --nodedir=/path/to/6.10.3/nodejs configure
```

Note that \-\-target and \-\-nodedir are only needed if compile for a specific nodejs version used on the device.

## build

```
# ./node_modules/node-pre-gyp/bin/node-pre-gyp build
```

When build completed, the generated binary can be found at ./lib/binding/node-v48-android-arm/node_uws.node