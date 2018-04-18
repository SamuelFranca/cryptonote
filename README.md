# Cycluscoin 

## About

Welcome to the repository of Cycluscoin. Here you will find source code, instructions, wiki resources, and integration tutorials.

Contents
* Building on Linux 64-bit
* Building on Mac OSX
* Building on Windows

## Building on Linux 64-bit

All commands below are adapted for Ubuntu, other distributions may need an other command set.

### Building with standard options

Create directory `dev_cycluscoin` somewhere and go there:
```
$> mkdir dev_cycluscoin
$> cd dev_cycluscoin
```

To go futher you have to have a number of packages and utilities.

* `build-essential` package:
    ```
    $> sudo apt-get install build-essential
    ```

* CMake (3.5 or newer):
    ```
    $> sudo apt-get install cmake
    $> cmake --version
    ```
    If version is too old, follow instructions on [the official site](https://cmake.org/download/).

* Boost (1.62 or newer):
    ```
    $> sudo apt-get install libboost-all-dev
    $> cat /usr/include/boost/version.hpp | grep "BOOST_LIB_VERSION"
    ```
    If version is too old, download boost from [boost.org](https://boost.org), unpack it into a folder inside `dev_cycluscoin` and rename it from `boost_1_66_0` or similar to just `boost`
    Build boost
    ```
    $> cd boost
    $dev_cycluscoin/boost> ./bootstrap.sh
    $dev_cycluscoin/boost> ./b2 link=static -j 8 --build-dir=build64 --stagedir=stage
    cd ..
    ```

Git-clone (or git-pull) cycluscoin source code in that folder:
```
$dev_cycluscoin> git clone https://github.com/SamuelFranca/cycluscoin
```

Put LMDB source code in `dev_cycluscoin` folder (source files are referenced via relative paths, so you do not need to separately build it):
```
$dev_cycluscoin> git clone https://github.com/LMDB/lmdb.git
```

Create build directory inside cycluscoin, go there and run CMake and Make:
```
$dev_cycluscoin> mkdir cycluscoin/build
$dev_cycluscoin> cd cycluscoin/build
$dev_cycluscoin/cycluscoin/build> cmake -DUSE_SSL=0 ..
$dev_cycluscoin/cycluscoin/build> time make -j4
```

Check built binaries by running them from `./dev_cycluscoin/cycluscoin/build` folder
```
$dev_cycluscoin/cycluscoin/build> ../release/src/cycluscoind -v
```

## Building on Mac OSX

### Building with standard options (10.11 El Capitan or newer)

You need command-line tools. Either get XCode from an App Store or run 'xcode-select --install' in terminal and follow instructions. First of all, you need [Homebrew](https://brew.sh).

Then open terminal and install CMake and Boost:

* `brew install cmake`
* `brew install boost`

Create directory `dev_cycluscoin` somewhere and go there:
```
$~/Downloads> mkdir <path-to-dev_cycluscoin-folder>
$~/Downloads> cd <path-to-dev_cycluscoin-folder>
```

Git-clone (or git-pull) cycluscoin source code in that folder:
```
$dev_cycluscoin> git clone https://github.com/SamuelFranca/cycluscoin
```

Put LMDB source code in `dev_cycluscoin` folder (source files are referenced via relative paths, so you do not need to separately build it):
```
$dev_cycluscoin> git clone https://github.com/LMDB/lmdb.git
```

Create build directory inside cycluscoin, go there and run CMake and Make:
```
$dev_cycluscoin> mkdir cycluscoin/build
$dev_cycluscoin> cd cycluscoin/build
$dev_cycluscoin/cycluscoin/build> cmake -DUSE_SSL=0 ..
$dev_cycluscoin/cycluscoin/build> time make -j4
```

Check built binaries by running them from `../bin` folder:
```
$dev_cycluscoin/cycluscoin/build> ../bin/cycluscoind -v
```

### Building with specific options

Binaries linked with Boost installed by Homebrew will work only on your computer's OS X version or newer, but not on older versions like El Capitan.

If you need binaries to run on all versions of OS X starting from El Capitan, you need to build boost yourself targeting El Capitan SDK.

Download [Mac OSX 10.11 SDK](https://github.com/phracker/MacOSX-SDKs/releases) and unpack to it into `Downloads` folder

Download and unpack [Boost](https://boost.org) to `Downloads` folder.

Then build and install Boost:
```
$~> cd ~/Downloads/boost_1_58_0/
$~/Downloads/boost_1_58_0> ./bootstrap.sh
$~/Downloads/boost_1_58_0> ./b2 -a -j 4 cxxflags="-stdlib=libc++ -std=c++14 -mmacosx-version-min=10.11 -isysroot/Users/user/Downloads/MacOSX10.11.sdk" install`
```

## Building on Windows

You need Microsoft Visual Studio Community 2017. [Download](https://www.visualstudio.com/vs/) and install it selecting `C++`, `git`, `cmake integration` packages.
Run `Visual Studio x64 command prompt` from start menu.

Create directory `dev_cycluscoin` somewhere:
```
$C:\> mkdir dev_cycluscoin
$C:\> cd dev_cycluscoin
```

Get [Boost](https://boost.org) and unpack it into a folder inside `dev_cycluscoin` and rename it from `boost_1_66_0` or similar to just `boost`.

Build boost (build 32-bit boost version only if you need 32-bit cycluscoin binaries).
```
$> cd boost
$C:\dev_cycluscoin\boost> bootstrap.bat
$C:\dev_cycluscoin\boost> b2.exe address-model=64 link=static -j 8 --build-dir=build64 --stagedir=stage
$C:\dev_cycluscoin\boost> b2.exe address-model=32 link=static -j 8 --build-dir=build32 --stagedir=stage32
cd ..
```

Git-clone (or git-pull) cycluscoin source code in that folder:
```
$C:\dev_cycluscoin> git clone https://github.com/SamuelFranca/cycluscoin
```

Put LMDB in the same folder (source files are referenced via relative paths, so you do not need to separately build it):
```
$C:\dev_cycluscoin> git clone https://github.com/LMDB/lmdb.git
```

Now launch Visual Studio, in File menu select `Open Folder`, select `C:\dev_cycluscoin\cycluscoin` folder.
Wait until CMake finishes running and `Build` appears in main menu.
Select `x64-Debug` or `x64-Release` from standard toolbar, and then `Build/Build Solution` from the main menu.

To build, change to a directory where this file is located, and run theas commands: 
```
mkdir build
cd build
cmake -G "Visual Studio 12 Win64" ..
```

And then do Build.
Good luck!