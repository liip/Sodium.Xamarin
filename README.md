# Sodium.Xamarin  [![Build status](https://ci.appveyor.com/api/projects/status/8qr4xp97e6upjkuu?svg=true)](https://ci.appveyor.com/project/jschmid/sodium-xamarin) [![NuGet Version](http://img.shields.io/nuget/v/Sodium.Xamarin.svg)](https://www.nuget.org/packages/Sodium.Xamarin) [![License](http://img.shields.io/badge/license-MIT-green.svg)](https://github.com/liip/Sodium.Xamarin/blob/master/LICENSE)

Sodium.Xamarin is a wrapper library for [libsodium](https://www.libsodium.org) specifically targeting Xamarin (Native or Forms).

## Why

None of the available libraries out there are compatible with Xamarin. The goal of this library is to take an existing .NET library and apply the minimal changes to make it compatible.

## Installation

### NuGet package

Install the [Sodium.Xamarin](https://www.nuget.org/packages/Sodium.Xamarin/) NuGet package in your Xamarin core project.

### Compile the native libraries

Installing the NuGet is not enough. You will have to compile the libsodium library for iOS and Android yourself, because they are not provided out of the box.

*The following instructions have been tested on Mac OS 10.14, using libsodium 1.0.18 and the [installation](https://download.libsodium.org/doc/installation) documentation.*

[Download the latest stable version](https://download.libsodium.org/libsodium/releases/) (for example `libsodium-1.0.18-stable.tar.gz`) and untar it.

From the root folder, run the commands:

```bash
./configure
make && make check
sudo make install
```

Make sure everything went fine.

#### Compile and install on iOS

Run the command:

```bash
LIBSODIUM_FULL_BUILD=true dist-build/ios.sh
```

This will create a new folder *libsodium-ios* that contains the *libsodium.a* file.

In Visual Studio - in the Xamarin.iOS project - add this new file in your project. Make sure that the *Build Action* of the file is *None*. Right-click in the iOS project and do  "Add > Add Native Reference". Select the file you have just added.

#### Compile and install on Android

First, make sure that you have installed the [Android NDK](https://developer.android.com/ndk/), and that you have the `ANDROID_NDK_HOME` environment variable. For example using:

```bash
export ANDROID_NDK_HOME=/Users/john/Library/Android/sdk/ndk/20.0.5594570
```

Then run the commands:

```bash
dist-build/android-x86.sh
dist-build/android-x86_64.sh
dist-build/android-armv7-a.sh
dist-build/android-armv8-a.sh
```

This will create 4 new folders that each contain the *libsodium.so* file. Beware of using the **.so** file and not the **.a** file like you did for iOS.

In you Xamarin.Android project create a new folder `Resources/lib/`. Inside, create 4 new folders called `x86`, `x86_64`, `armeabi-v7a` and `arm64-v8a`. Inside those folders add the *libsodium.so* files following the mapping:

| libsodium folder           | Xamarin.Android folder |
| -------------------------- | ---------------------- |
| libsodium-android-i686     | x86                    |
| libsodium-android-westmere | x86_64                 |
| libsodium-android-armv7-a  | armeabi-v7a            |
| libsodium-android-armv8-a  | arm64-v8a              |

Change the *Build Action* of the files to *AndroidNativeLibrary*.

## Usage

You can now use libsodium inside your Xamarin code, by including the package `Sodium`. For example:

```csharp
var version = Sodium.SodiumCore.SodiumVersionString();
Console.WriteLine($"Libsodium version: {version}");
```

Use the documentation from [libsodium](https://download.libsodium.org/) and [libsodium-net](http://bitbeans.gitbooks.io/libsodium-net/content/).

## Acknowledgements

This library is based on [libsodium-core](https://github.com/tabrath/libsodium-core), which itself is based on [libsodium-net](https://github.com/adamcaudill/libsodium-net), which uses [libsodium](https://www.libsodium.org). The present work would not have been possible without the hard work of all maintainers. 

## License

The library and it's parents are subject to the MIT license (see [LICENSE](LICENSE)). Libsodium is subject to the [ISC license](https://en.wikipedia.org/wiki/ISC_license).
