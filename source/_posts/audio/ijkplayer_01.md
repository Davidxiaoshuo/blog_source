---
title: IjkPlayer 从编译集成到简单音频播放器的封装 [Android & iOS] 一
categories: Android
tags: [android, audio]
---

## 编译篇【Android】

### 背景
IjkPlayer 是 `bilibili` 团队出品的一款基于 FFmpeg 的轻量级的音视频播放器。 <br>
官方地址： [ijkplayer](https://github.com/bilibili/ijkplayer)  <br>
由于我所在项目需要一款音频播放器，所以后面都是基于这个大前提来做的。

### 环境准备
- 编译环境：macOS
- 所需要工具：homebrew 【软件包管理器】, git 【分散式版本控制工具】, yasm 【汇编编译器】

```bash
# 安装 homebrew
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# 安装 git
brew install git

# 安装 yasm
brew install yasm
```

- 配置环境变量：

``` bash
# add these lines to your ~/.bash_profile or ~/.profile or ~/.zshrc (取决于你使用的终端工具)
export ANDROID_SDK=<your sdk path>
export ANDROID_NDK=<your ndk path> 【最好区别于你的开发环境变量配置】
```

> **Note： 由于 ijkplayer 代码比较古老了，NDK 环境最好选择 `14 ` 版本。 NDK 下载[链接](https://developer.android.com/ndk/downloads/older_releases.html#ndk-14b-downloads)**

### 编译

- 通过  `git` 将 ijkplayer 源码 clone 到本地

```bash
git clone https://github.com/bilibili/ijkplayer.git <you local path>
```

- 下载 FFmpeg

```bash
./init-android.sh
```

- 下载 OpenSSL, 增加 `https` 支持

```bash
./init-android-openssl.sh
```

- 编译 OpenSSL

```bash
cd android/contrib

./compile-openssl.sh clean

./compile-openssl.sh all
```

- 编译 FFmpeg

```bash
cd android/contrib

./compile-ffmpeg.sh clean

./compile-ffmpeg.sh all
```

- 编译 ijkplayer

```
cd android

./compile-ijk.sh clean

./compile-ijk.sh all
```


> **Note: 编译说明：** <br>
>
> `compile-openssl.sh` & `compile-ffmpeg.sh` & `compile-ijk.sh` 后面的参数代表要变哪个CPU架构的版本，默认是 `armv7a`；<br>
>  ```
armv5 armv7a arm64 x86 x86_64(指定编译哪个版本)
all32（所有32位处理器版本，包含armv5 armv7a x86
all （所有通用版本，包含armv5 armv7a arm64 x86 x86_64)
clean （清除之前编译的缓存）
check (检测支持的版本) 
```


### 编译 FFmpeg 说明

编译 ffmpeg 时，根据自己的需求来确定你需要的是更多的 `codec/format`, 还是轻量级的 `codec/format` 就可以了。【官方文档有描述。后面将会专门讲配置特定需求的 codec/format】

- if you  prefer more codec/format

```
cd config
rm module.sh
ln -s module-default.sh module.sh
cd android/contrib
# cd ios
sh compile-ffmpeg.sh clean
```

- if you prefer less codec/format for smaller binary size (include hevc function) 【hevc: HEVC是High Efficiency Video Coding的缩写，是一种新的视频压缩标准，用来以替代H.264/AVC编码标准，2013年1月26号，HEVC正式成为国际标准。】

```
cd config
rm module.sh
ln -s module-lite-hevc.sh module.sh
cd android/contrib
# cd ios
sh compile-ffmpeg.sh clean
```

- if you prefer less codec/format for smaller binnary size (by default)

```
cd config
rm module.sh
ln -s module-lite.sh module.sh
cd android/contrib
# cd ios
sh compile-ffmpeg.sh clean
```

## 编译中遇到的问题

### NDK 版本引起的问题，***强烈建议选择 `r14b` 版本，上面有 ndk 下载链接***

- 确认你的 NDK 版本是否大于等于 r10e

```
cat $ANDROID_NDK/source.properties

Pkg.Desc = Android NDK
Pkg.Revision = 14.1.3816874 // 我的NDK版本
```

- 确定了你的 NDK 版本符合要求，还是报了如下错误

```
You need the NDKr10e or later
```

那么你就要需要修改如下代码, 把你的版本添加进版本检查中：

```
cd android/contrib/tool

vim do-detect-env.sh

IJK_NDK_REL=$(grep -o '^Pkg\.Revision.*=[0-9]*.*' $ANDROID_NDK/source.properties 2>/dev/null | sed 's/[[:space:]]*//g' | cut -d "=" -f 2)
echo "IJK_NDK_REL=$IJK_NDK_REL"
case "$IJK_NDK_REL" in
    11*|12*|13*|14*|15*|20*)
        if test -d ${ANDROID_NDK}/toolchains/arm-linux-androideabi-4.9
        then
            echo "NDKr$IJK_NDK_REL detected"
        else
            echo "You need the NDKr10e or later"
            exit 1
        fi
    ;;
    *)
        echo "You need the NDKr10e or later"
        exit 1
    ;;

```

- 由于 ndk 兼容问题导致 standalone toolchain， 如果 `macOS` 遇到这个问题，可以将使用 `r14b` 版本的 NDK； 参考[链接](https://github.com/Bilibili/ijkplayer/issues/3378)

```
====================
[*] check archs
====================
FF_ALL_ARCHS = armv5 armv7a arm64 x86 x86_64
FF_ACT_ARCHS = armv5 armv7a arm64 x86 x86_64


--------------------
[*] make NDK standalone toolchain
--------------------
build on Darwin x86_64
ANDROID_NDK=/Users/davidxiaoshuo/Documents/dev_tools/android/sdk/ndk-bundle
IJK_NDK_REL=20.0.5594570
NDKr20.0.5594570 detected

--------------------
[*] make NDK standalone toolchain
--------------------
build on Darwin x86_64
ANDROID_NDK=/Users/davidxiaoshuo/Documents/dev_tools/android/sdk/ndk-bundle
IJK_NDK_REL=20.0.5594570
NDKr20.0.5594570 detected
HOST_OS=darwin
HOST_EXE=
HOST_ARCH=x86_64
HOST_TAG=darwin-x86_64
HOST_NUM_CPUS=12
BUILD_NUM_CPUS=24
Auto-config: --arch=arm
ERROR: Failed to create toolchain.

```

### 其他方面导致的问题

- 编译模型脚本 （module.sh）导致的问题

```
WARNING: arm-linux-androideabi-pkg-config not found, library detection may fail.

--------------------
[*] compile ffmpeg
--------------------
In file included from ./libavutil/internal.h:42:0,
                 from ./libavutil/common.h:467,
                 from ./libavutil/avutil.h:296,
                 from ./libavutil/opt.h:31,
                 from libavfilter/af_adelay.c:22:
./libavutil/timer.h:38:31: fatal error: linux/perf_event.h: No such file or directory
 # include <linux/perf_event.h>
                               ^
compilation terminated.
In file included from ./libavutil/internal.h:42:0,
                 from ./libavutil/common.h:467,
                 from ./libavutil/avutil.h:296,
                 from libavfilter/avfilter.h:41,
                 from libavfilter/audio.h:25,
                 from libavfilter/af_acopy.c:19:
./libavutil/timer.h:38:31: fatal error: linux/perf_event.h: No such file or directory
 # include <linux/perf_event.h>
```
可以在 `module.sh` 最后添加 `export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --disable-linux-perf"` 这行代码。 参考[链接](https://github.com/Bilibili/ijkplayer/issues/4043)



