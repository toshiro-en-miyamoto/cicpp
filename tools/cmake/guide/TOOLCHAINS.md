# CMake Toolchains

[CMake uses a toolchain](https://cmake.org/cmake/help/latest/manual/cmake-toolchains.7.html) of utilities to compile, link libraries and create archives, and other tasks to drive the build. The toolchain utilities available are determined by the languages enabled. In normal builds, CMake automatically determines the toolchain for host builds based on system introspection and defaults.

## Languages

Languages are enabled by the `project()` command. Language specific built-in variables, such as `CMAKE_CXX_COMPILER` are set by invoking the `project()` command.

```cmake
project(CXX_Only_Project CXX)
```

When a language is enabled, CMake finds a compiler for that language, and determines some information, such as the vendor and version of the compiler, the target architecture and bit-width, the location of corresponding utilities etc.

## Cross Compiling

If `cmake` is invoked with the command line parameter

- `--toolchain path/to/file` or
- `-DCMAKE_TOOLCHAIN_FILE=path/to/file`

the file will be loaded early to set values for the compilers.

### Cross Compiling for Linux

```cmake
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)

set(CMAKE_SYSROOT /home/devel/rasp-pi-rootfs)
set(CMAKE_STAGING_PREFIX /home/devel/stage)

set(tools /home/devel/gcc-4.7-linaro-rpi-gnueabihf)
set(CMAKE_C_COMPILER ${tools}/bin/arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER ${tools}/bin/arm-linux-gnueabihf-g++)

set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
```

Where:

- `CMAKE_SYSTEM_NAME`: is the CMake-identifier of the target platform to build for.
- `CMAKE_SYSTEM_PROCESSOR`: is the CMake-identifier of the target architecture.
- `CMAKE_SYSROOT`: is optional, and may be specified if a sysroot is available.
- `CMAKE_STAGING_PREFIX`: is also optional. It may be used to specify a path on the host to install to.
- `CMAKE_<LANG>_COMPILER`: variable may be set to full paths, or to names of compilers to search for in standard locations.

CMake `find_*` commands will look in the sysroot, and the `CMAKE_FIND_ROOT_PATH_*` entries by default in all cases, as well as looking in the host system root prefix.

### Cross Compiling using Clang

The `CMAKE_<LANG>_COMPILER_TARGET` can be set to pass a value to those supported compilers when compiling:

```cmake
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)

set(triple arm-linux-gnueabihf)

set(CMAKE_C_COMPILER clang)
set(CMAKE_C_COMPILER_TARGET ${triple})
set(CMAKE_CXX_COMPILER clang++)
set(CMAKE_CXX_COMPILER_TARGET ${triple})
```
