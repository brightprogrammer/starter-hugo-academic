---
title: Game Engine Series - 0x0003
subtitle: Simple SDL Window Setup
date: 2021-09-15T15:20:00.372Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
In this post, we will setup our first window, that will present our rendered images on the screen. We will use [SDL](https://libsdl.org/) for this. You can use other windowing libraries like [GLFW](https://glfw.org/), [Qt](https://qt.io/), etc... This dependes on what functionality and how much of it you want and the most important factor of them all is how much you are comfortable with the API of the library/utility you are using. I'm comfortable with [SDL](https://libsdl.org/), plus, [Unreal Engine](https://www.unrealengine.com/en-US/) uses it too. One more plus point with SDL is that it runs of many platforms : 

> SDL officially supports **Windows, Mac OS X, Linux, iOS, and Android**.

This almost all you want when you are beginning to make a Game Engine. For now I will do development for the specific platform I am working on (Linux). This doesn't mean that our engine won't work on other platforms, instead it just means that some of the platform dependent tasks we are doing in our Engine "might" not work. For example : If we don't use [SDL](https://libsdl.org/) to manage the platform dependent tasks for a window creation then for now our engine will be able display rendered images only on Linux. We can do some off-screen rendering on a non supported to output the rendered images as a video / GIF / screenshot etc... but that won't be fulfill the purpose of a Game Engine. 

There is a very nice [set of tutorials](https://lazyfoo.net/tutorials/SDL/) available for [SDL](https://libsdl.org/) on [lazyfoo](https://lazyfoo.net/). You can try reading that if you don't understand some part of this tutorial.

So, we will begin by introducing necessary includes in our `main` source file.

```cpp
// stdlib includes
#include <iostream>

// sdl includes
#include <SDL2/SDL.h>
#include <SDL2/SDL_vulkan.h>

// vulkan includes

int main(){
    puts("Infinity Engine [ Version - 0.0 ]");
    return 0;
}
```

The `SDL2/SDL.h` is for [SDL](https://libsdl.org/) functions and ...

```cpp
// stdlib includes
#include <SDL2/SDL_error.h>
#include <SDL2/SDL_video.h>
#include <cstdlib>
#include <iostream>

// sdl includes
#include <SDL2/SDL.h>
#include <SDL2/SDL_vulkan.h>

// vulkan includes

// typedefs
typedef uint64_t uint64;
typedef uint32_t uint32;
typedef uint16_t uint16;
typedef uint8_t uint8;

// global data
constexpr const char* appName           = "Infinity Engine";
constexpr int mainWindowWidth           = 800;
constexpr int mainWindowHeight          = 600;
constexpr uint32 windowCreationFlags    = SDL_WINDOW_VULKAN | SDL_WINDOW_SHOWN; 

int main(){
    puts("Infinity Engine [ Version - 0.0 ]");

    // initialize sdl video subsystem
    if(!SDL_WasInit(SDL_INIT_VIDEO)) SDL_InitSubSystem(SDL_INIT_VIDEO);

    // pointer to sdl window
    SDL_Window* window = nullptr;

    // create window
    window = SDL_CreateWindow("Infinity Engine", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, 
                               mainWindowWidth, mainWindowHeight, windowCreationFlags);

    // check if window was created
    if(window == nullptr){
        std::cout << "Window creation failed : " << SDL_GetError() << std::endl;
    }

    return EXIT_SUCCESS;
}
```

WORK IN PROGRESS