---
title: Game Engine Series 0x0001
subtitle: Project Setup & Intro To Vulkan
date: 2021-09-10T13:10:16.921Z
draft: false
featured: false
tags:
  - GameEngine
  - ComputerGraphics
  - C/C++
  - Series
categories:
  - GameEngine
  - Series
  - ComputerGraphics
projects: []
image:
  filename: featured.jpg
  focal_point: Smart
  preview_only: false
---
In this post we will setup our project directory for development. There is no *hard and fast rule* to setup project directory structure and this one will be based on my past experiences. We will be using the following things (for now) : 

* [Vulkan Graphics API](https://www.vulkan.org/)
* [SDL (Simple DirectMedia Layer)](https://www.libsdl.org)
* C++ Compiler (Clang or GCC)
* [CMake Build System](https://cmake.org/)
* A Good Text Editor (I'll be using [VSCode](https://code.visualstudio.com/download))

The list may grow in future for things like allocators, logging systems, image loader, 3D model loader etc...

I plan to go in full depth of almost all topics that I think will be difficult to understand for new readers. At the time of writing this I am a beginner in Vulkan too and there are some concepts that I lack. In the pursuit of making this blog better and better (as this represents me) I will learn a lot of things and write about what I learned. This will be helpful to readers in a way that they will be learning this in the easiest language possible.

I planned various projects and they never got *that far* because of lack of motivation, but this time I'm motivated to make this series long enough for me to take the engine to a mature state and for you to learn Vulkan and see it's power.

Let's begin by creating the project folder. I will name my engine **Infinity**. The project directory structure will look something like this : 

* infinity

  * include
  * source

    * include
    * source
    * CMakeLists.txt
  * build
  * trash
  * lib
  * share
  * bin
  * dependencies
  * deps.sh
  * CMakeLists.txt

Below I will explain the meaning of each file and folder : 

* infinity (/) - project root directory
* /include - contains headers of dependencies
* /source - contains our actual project code
* /source/include - contains project header files (.hpp)
* /source/source - contains project source files (.cpp)
* /source/CMakeLists.txt - defines our libraries and declares that source is a subdirectory of main project
* /build - contains our build files
* /trash - contains useless code (but some of the code snippets might be useful)
* /lib - contains built library files of our dependencies
* /share - contains file corresponding to linux's share directory
* /bin - contains binaries of built dependencies
* /dependencies - contains git submodules
* /deps.sh - script to build our dependencies and place the build files in their corresponding location
* /CMakeLists.txt - the main CMake script for our project

Even if you don't create folders include, lib, share and bin, they will automatically be created by `deps.sh` script. Note that `deps.sh` will only work on Unix based OS. So, you will have to convert it to a windows script. 

Next, we will setup the `/CMakeLists.txt`  and try to compile a simple `main.cpp`. Below is what the `CMakeLists.txt` will look like.

```cmake
# cmake min version
cmake_minimum_required(VERSION 3.5)

# project settings
project(infinity VERSION 0 LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories("lib")

# subdirectories setup
add_subdirectory("source")
```

and the `/source/CMakeLists.txt` will contain : 

```cmake
# main executable
add_executable(${PROJECT_NAME} "source/main.cpp")
```

After this create a `/source/source/main.cpp` file and enter the following contents into it :

```cpp
#include <iostream>

int main(){
    // print something
    puts("infinity engine version 0.0");

    return 0;
}
```

If you are using VSCode like me then you can do `Ctrl`+`Shift`+`P` and search for `CMake: Configure` to generate build files