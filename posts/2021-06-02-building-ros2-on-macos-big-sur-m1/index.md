---
permalink: /posts/building-ros2-on-macos-big-sur-m1/
title: "How to build and install ROS2 on macOS Big Sur M1"
author: "Kliment Mamykin"
date: "2021-06-02"
image: ''
tags:
- macOS
- ROS2
- robotics
- Apple Silicon
- M1
---

This is a detailed list of instructions with workarounds to build a ROS2 distribution (galactic in this case) on the latest Apple's macOS (Big Sur with Apple Silicon M1 processor). Not every tool has been tested on Big Sur+M1 and some tools have been disabled in the build as temp workarounds.

For the current status of the known issues the progress is tracked [HERE](https://github.com/ros2/ros2/issues/1148).

In general this article follows the build instructions of [galactic ROS2 release](https://docs.ros.org/en/galactic/Installation/macOS-Development-Setup.html). The process is modified to add Python virtual environment, direnv, and various workarounds to fix the current issues. No System Integrity Protection (SIP) disabling is required, and the only global footprint on the machine is the packages installed through Homebrew.

## Directory layout

```
<root>                              - the root directory for this experiment
  ├── .envrc                        - direnv file to setup env vars, source various files etc
  ├── .venv/                        - local Python virtual environment
  ├── ros2_galactic/                - ROS2 workspace, most of the commands will be run here
  │   ├── {src|build|install|log}   - various directories 
  │
  ├── Pillow/                       - not ROS related packages, to build or experiment out of ROS workspace
  └── numpy/                        - another out of ROS workspace repo
```

## Homebrew

Homebrew is installed into a different location `/opt/homebrew` on Apple Silicon Mac. But in order to be compatible with Intel based machines, it's best to use `$(brew --prefix)` expansion to get the location of Homebrew installed packages.

## direnv

`direnv` is a tool to manage separate shell environments per project. `cd` into a directory executes `.envrc` file and captures all exported environment variables. `cd` out of the directory and the shell environment is reverted. This is perfect to automatically source Python virtual env or ROS setup scripts.

`brew install direnv` and follow the instructions to update your shell dot files (.bashrc or .zshrc).

## Python virtual environment

Inside the <root> directory:

```
# python3 --version
Python 3.9.5
# python3 -m venv .venv
# echo "source .venv/bin/activate \nunset PS1" >> .envrc
direnv: error .../.envrc is blocked. Run `direnv allow` to approve its content
# direnv allow
# which python
<root>/.venv/bin/python
# which pip
<root>/.venv/bin/pip
# pip install --upgrade pip && pip list
...
Package    Version
---------- -------
pip        21.1.2
setuptools 56.0.0
```

We are now in a prestene Python virtual environment.

## Install prerequisites

Follow "Install prerequisites" from https://docs.ros.org/en/galactic/Installation/macOS-Development-Setup.html

Except `export OPENSSL_ROOT_DIR=$(brew --prefix openssl)` could go to the `.envrc` file.
Add `export Qt5_DIR=$(brew --prefix qt@5)/lib/cmake` to `.envrc` as well.

There is no need to set `export CMAKE_PREFIX_PATH` at this time because later on we will pass it as an explicit parameter to `colcon`.

Install Python packages:

```
pip install -U \
 argcomplete catkin_pkg colcon-common-extensions coverage \
 cryptography empy flake8 flake8-blind-except flake8-builtins \
 flake8-class-newline flake8-comprehensions flake8-deprecated \
 flake8-docstrings flake8-import-order flake8-quotes ifcfg \
 importlib-metadata lark-parser lxml mock mypy netifaces \
 nose pep8 pydocstyle pydot pygraphviz pyparsing \
 pytest-mock rosdep setuptools vcstool psutil rosdistro
```

Note, `matplotlib` is missing from this list, requires a few extra steps.

I also installed [rosinstall-generator](http://wiki.ros.org/rosinstall_generator) (but this is optional).

```
pip install rosinstall-generator
```

#### Build numpy and Pillow from source

https://stackoverflow.com/questions/66204654/unable-to-install-matplotlib-with-python3-on-m1-mac-using-pip3

Note that the location of numpy and Pillow directories must be outside of the `ros2_galactic` directory. 

```
cd <root>
pip install Cython
git clone https://github.com/numpy/numpy.git
cd numpy
pip install . --no-binary :all: --no-use-pep517
cd ..
git clone https://github.com/python-pillow/Pillow.git
cd Pillow
pip install . --no-binary :all: --no-use-pep517
```

and finally install matplotlib

```
# pip install matplotlib
...there will be some errors here but matplotlib installs successfully...
# pip list | grep matplotlib
matplotlib                 3.4.2
```

## SIP

No need to disable System Integrity Protection (SIP)

## Get the ROS 2 code

No change from https://docs.ros.org/en/galactic/Installation/macOS-Development-Setup.html#get-the-ros-2-code

## Build the ROS 2 code

This is where the fun starts. I started with the following colcon build command:

```
colcon build \
  --symlink-install \
  --merge-install \
  --event-handlers console_cohesion+ console_package_list+ \
  --packages-skip-by-dep python_qt_binding \
  --cmake-args \
    --no-warn-unused-cli \
    -DBUILD_TESTING=OFF \
    -DINSTALL_EXAMPLES=ON \
    -DCMAKE_OSX_SYSROOT=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk \
    -DCMAKE_OSX_ARCHITECTURES="arm64" \
    -DCMAKE_PREFIX_PATH=$(brew --prefix):$(brew --prefix qt@5)
```

Let's take it apart. The build is done with [colcon](https://colcon.readthedocs.io/en/released/index.html) tool, which reads `package.xml`
from each repository installed in `src`, builds a DAG of dependencies, and then executes package builds in the correct topological order. 
`--symlink-install` created symlins inside the install directory. `--merge-install` creates a flatter install directory that results in shorter PATHs when sourcing install/setup.sh later on. `--event-handlers console_cohesion+ console_package_list+` gives a slightly better console logging. 

All `--cmake-args` are necessary to properly build the distro on Big Sur + M1. 

### Patches

On the failure to build ``

```
In file included from .../ros2_galactic/src/osrf/osrf_testing_tools_cpp/osrf_testing_tools_cpp/src/memory_tools/stack_trace.cpp:17:
In file included from .../ros2_galactic/src/osrf/osrf_testing_tools_cpp/osrf_testing_tools_cpp/src/memory_tools/./stack_trace_impl.hpp:31:
.../ros2_galactic/src/osrf/osrf_testing_tools_cpp/osrf_testing_tools_cpp/src/memory_tools/./vendor/bombela/backward-cpp/backward.hpp:3931:60: error: member reference type 'struct __darwin_mcontext64 *' is a pointer; did you mean to use '->'?
    error_addr = reinterpret_cast<void *>(uctx->uc_mcontext.pc);
                                          ~~~~~~~~~~~~~~~~~^
                                                           ->
```

patch `src/osrf/osrf_testing_tools_cpp/osrf_testing_tools_cpp/src/memory_tools/vendor/bombela/backward-cpp/backward.hpp`

```
--- a/src/memory_tools/vendor/bombela/backward-cpp/backward.hpp
+++ b/src/memory_tools/vendor/bombela/backward-cpp/backward.hpp
@@ -3927,8 +3927,10 @@ public:
     error_addr = reinterpret_cast<void *>(uctx->uc_mcontext.gregs[REG_EIP]);
 #elif defined(__arm__)
     error_addr = reinterpret_cast<void *>(uctx->uc_mcontext.arm_pc);
-#elif defined(__aarch64__)
+#elif defined(__aarch64__) && !defined(__APPLE__)
     error_addr = reinterpret_cast<void *>(uctx->uc_mcontext.pc);
+#elif defined(__APPLE__) && defined(__aarch64__)
+    error_addr = reinterpret_cast<void *>(uctx->uc_mcontext->__ss.__pc);
 #elif defined(__mips__)
     error_addr = reinterpret_cast<void *>(reinterpret_cast<struct sigcontext*>(&uctx->uc_mcontext)->sc_pc);
 #elif defined(__ppc__) || defined(__powerpc) || defined(__powerpc__) ||        \
```

On the failure to build `rviz_ogre_vendor`

```
Undefined symbols for architecture x86_64:
  "_FT_Done_FreeType", referenced from:
      Ogre::Font::loadResource(Ogre::Resource*) in OgreFont.cpp.o
  "_FT_Init_FreeType", referenced from:
      Ogre::Font::loadResource(Ogre::Resource*) in OgreFont.cpp.o
  "_FT_Load_Char", referenced from:
      Ogre::Font::loadResource(Ogre::Resource*) in OgreFont.cpp.o
  "_FT_New_Memory_Face", referenced from:
      Ogre::Font::loadResource(Ogre::Resource*) in OgreFont.cpp.o
  "_FT_Set_Char_Size", referenced from:
      Ogre::Font::loadResource(Ogre::Resource*) in OgreFont.cpp.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make[5]: *** [lib/macosx/libOgreOverlay.1.12.1.dylib] Error 1
make[4]: *** [Components/Overlay/CMakeFiles/OgreOverlay.dir/all] Error 2
make[4]: *** Waiting for unfinished jobs....
make[3]: *** [all] Error 2
make[2]: *** [ogre-v1.12.1-prefix/src/ogre-v1.12.1-stamp/ogre-v1.12.1-build] Error 2
make[1]: *** [CMakeFiles/ogre-v1.12.1.dir/all] Error 2
make: *** [all] Error 2
```

patch `src/ros2/rviz/rviz_ogre_vendor/CMakeLists.txt`

```
diff --git a/CMakeLists.txt b/CMakeLists.txt
index faac7e1b..c36877c3 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -120,7 +120,7 @@ macro(build_ogre)
     set(OGRE_CXX_FLAGS "${OGRE_CXX_FLAGS} /w /EHsc")
   elseif(APPLE)
     set(OGRE_CXX_FLAGS "${OGRE_CXX_FLAGS} -std=c++14 -stdlib=libc++ -w")
-    list(APPEND extra_cmake_args "-DCMAKE_OSX_ARCHITECTURES='x86_64'")
+    list(APPEND extra_cmake_args "-DCMAKE_OSX_ARCHITECTURES='arm64'")
   else()  # Linux
     set(OGRE_C_FLAGS "${OGRE_C_FLAGS} -w")
     # include Clang -Wno-everything to disable warnings in that build. GCC doesn't mind it
```

On another failure to build `rviz_ogre_vendor`

```
In file included from /Users/kmamykin/Projects/ros2-build-on-macOS/ros2_galactic/build/rviz_ogre_vendor/ogre-v1.12.1-prefix/src/ogre-v1.12.1/OgreMain/src/OgreOptimisedUtilSSE.cpp:36:
In file included from /Users/kmamykin/Projects/ros2-build-on-macOS/ros2_galactic/build/rviz_ogre_vendor/ogre-v1.12.1-prefix/src/ogre-v1.12.1/OgreMain/src/OgreSIMDHelper.h:69:
In file included from /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/12.0.5/include/xmmintrin.h:13:
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/12.0.5/include/mmintrin.h:33:5: error: use of undeclared identifier '__builtin_ia32_emms'; did you mean '__builtin_isless'?
    __builtin_ia32_emms();
    ^
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX11.3.sdk/usr/include/c++/v1/math.h:649:12: note: '__builtin_isless' declared here
    return isless(__lcpp_x, __lcpp_y);
           ^
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX11.3.sdk/usr/include/math.h:545:22: note: expanded from macro 'isless'
#define isless(x, y) __builtin_isless((x),(y))
                     ^
```

update `build/rviz_ogre_vendor/ogre-v1.12.1-prefix/src/ogre-v1.12.1/OgreMain/include/OgrePlatformInformation.h` (which is downloaded during the build, so you need to try building rviz_ogre_vendor at least once).

```
--- build/rviz_ogre_vendor/ogre-v1.12.1-prefix/src/ogre-v1.12.1/OgreMain/include/OgrePlatformInformation.h.orig	2021-06-02 16:28:58.000000000 -0400
+++ build/rviz_ogre_vendor/ogre-v1.12.1-prefix/src/ogre-v1.12.1/OgreMain/include/OgrePlatformInformation.h	2021-06-02 16:30:50.000000000 -0400
@@ -50,11 +50,11 @@
 #   define OGRE_CPU OGRE_CPU_X86

 #elif OGRE_PLATFORM == OGRE_PLATFORM_APPLE && defined(__BIG_ENDIAN__)
 #   define OGRE_CPU OGRE_CPU_PPC
 #elif OGRE_PLATFORM == OGRE_PLATFORM_APPLE
-#   define OGRE_CPU OGRE_CPU_X86
+#   define OGRE_CPU OGRE_CPU_ARM
 #elif OGRE_PLATFORM == OGRE_PLATFORM_APPLE_IOS && (defined(__i386__) || defined(__x86_64__))
 #   define OGRE_CPU OGRE_CPU_X86
 #elif defined(__arm__) || defined(_M_ARM) || defined(__arm64__) || defined(__aarch64__)
 #   define OGRE_CPU OGRE_CPU_ARM
 #elif defined(__mips64) || defined(__mips64_)
```

## Testing the install

Once you have a successful build, you can test it, for example by running `rviz2`:

```
source ./install/setup.sh
./install/bin/rviz2
```

Enjoy!
