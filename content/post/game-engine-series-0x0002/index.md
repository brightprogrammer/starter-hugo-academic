---
title: Game Engine Series 0x0002
subtitle: Introduction To Vulkan
date: 2021-09-11T06:03:59.914Z
draft: false
featured: false
tags:
  - GameEngine
  - ComputerGraphics
  - Infinity
  - C++
categories:
  - GameEngine
  - ComputerGraphics
  - Infinity
image:
  filename: featured.jpg
  focal_point: Smart
  preview_only: false
---
image credit : ozzy10 from wallpapers.com

This post will be on introduction to Vulkan API. We will setup our initiialization code and make our new window. Let's begin!

### What is Vulkan ?

Vulkan is a fast, new and modern 3D graphics programming API. The initial release was about 5 years ago and it is now used my many games and game engines. The API was initially made by AMD for modern GPUs which was later made open source and is now maintained by the **Khronos Group**. It is written purely in C and supports many platforms like : [Android](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Android&stick=H4sIAAAAAAAAAONgVuLSz9U3MCqvKEkvX8TK7piXUpSfmQIAyAsAJhgAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoAXoECEIQAw), [Linux](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Linux&stick=H4sIAAAAAAAAAONgVuLUz9U3SCuoqipYxMrqk5lXWgEATgerNhUAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoAnoECEIQBA), [Fuchsia](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Fuchsia&stick=H4sIAAAAAAAAAONgVuLVT9c3NEw2zEnLKKoqXsTK7laanFGcmQgAxklZLRsAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoA3oECEIQBQ), [BSD Unix](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=BSD+Unix&stick=H4sIAAAAAAAAAONgVuLQz9U3MEwzzVnEyuEU7KIQmpdZAQDAUa93FwAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoBHoECEIQBg), [QNX](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=QNX&stick=H4sIAAAAAAAAAONgVuLQz9U3yEhKSlnEyhzoFwEA4JwBWRIAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoBXoECEIQBw), [Windows](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Windows&stick=H4sIAAAAAAAAAONgVuLQz9U3MCmKt1jEyh6emZeSX14MAFTjqsQWAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoBnoECEIQCA), [Nintendo Switch](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Nintendo+Switch&stick=H4sIAAAAAAAAAONgVuLVT9c3NEw2K7FMMzPMWcTK75eZV5Kal5KvEFyeWZKcAQB0QcE-IwAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoB3oECEIQCQ), [Stadia](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Stadia&stick=H4sIAAAAAAAAAONgVuLVT9c3NEzLMs8oyUiPX8TKFlySmJKZCACloW6EGgAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoCHoECEIQCg), [Tizen](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Tizen&stick=H4sIAAAAAAAAAONgVuLSz9U3yDDPqchKWcTKGpJZlZoHADhg-LkWAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoCXoECEIQCw), [macOS](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=macOS&stick=H4sIAAAAAAAAAONgVuLQz9U3MDWtLFrEypqbmOwfDADb5b_yFAAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoCnoECEIQDA), [IOS](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=IOS&stick=H4sIAAAAAAAAAONgVuLSz9U3MC5PyjE0WcTK7OkfDAC-b2NaFAAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoC3oECEIQDQ), [Raspberry Pi](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Raspberry+Pi&stick=H4sIAAAAAAAAAONgVuLSz9U3SM9NNzZLX8TKE5RYXJCUWlRUqRCQCQA5myHgHQAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoDHoECEIQDg). It uses [Apache License](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Apache+License&stick=H4sIAAAAAAAAAONgVuLUz9U3MDTKjrdcxMrnWJCYnJGq4JOZnJpXnAoAzGCpBx4AAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoAXoECEgQAw) 2.0 which means we can use it too.

### But why is it called an API?

Well, this is because it is an API. Vulkan is a set of rules defined for modern GPU vendors to help them make a good graphics driver. Vulkan defines a set functions which we can use to interact with GPU or any other device that can do some processing. The GPU vendors follow the rules defined by Vulkan and make graphics drivers (GPU driving code) for us so that we can interact with the GPU in the defined way. So, you see vulkan is not a library, instead it's just a set of rules and functions that obey those rules and since we use these functions to interact with GPU indirectly, it is called an interface and i.e why this is an API.

### Can't we just directly talk to GPU?

Short answer : **No**

Long answer : You cannot, because that'd mean the GPU vendor revelaing their secrets to you. Each graphic card manufacturer try to make their product faster than their competetors and in that pursuit they research a lot and make their own methods to do stuffs faster and those methods are to be kept confidential. That is why we have a graphics driver. It is a platform and graphics card dependent library which contains the functions that [Vulkan Spec](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html) defines and we link to that library to interact with the GPU.

### But, how exactly do we talk to GPU?

As stated above, we link our program with the graphics driver in order to make it run. But wouldn't that mean that we have to have graphics driver for every vendor our there? No, you don't need to do that! This is where the interface comes in. Since, Vulkan is an interface between our program and GPU, as long as both the hardware vendor and the developer follow the rules defined by Vulkan to use it's interface, then they don't need to care about each other. You can just assume that the system that your program will run in will already have the graphics drivers in place and then we can just link to it dynamically at runtime because ofcourse static linking will not be possible here. 

### How exactly will I link to driver dynamically?

There are two ways to do this! Either you make our own loader to dynamically link to the libraries like what [volk](https://github.com/zeux/volk) does or you use the [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader) developed by the Khronos Group. For help with the former method, refer to the following resources (if you want to make your own loader instead of using [volk](https://github.com/zeux/volk) or [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader)) : 

* For Linux or other Unix based systems : <https://tldp.org/HOWTO/Program-Library-HOWTO/dl-libraries.html>
* For Windows : <https://docs.microsoft.com/en-us/windows/win32/dlls/using-run-time-dynamic-linking>
* Intel tutorial on how to do it for vulkan : <https://software.intel.com/content/www/us/en/develop/articles/api-without-secrets-introduction-to-vulkan-part-1.html>

For the latter method, we just have to download [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader) and link to it statically or dynamically however you like. This is the method I will chose. There is a small performance gain if you use a custom loader instead, I, however at this moment just want to see something on my screen ASAP.

### How does the loader work?

The main task of loader is to search for the graphics driver, also called `Installable Client Driver` (`ICD`) and then get pointers to all or some functions from the library file after loading it into memory. Say you are calling some function named `doSomething(xyz)`. Now, this function has to be defined somewhere otherwise your program will show a `segfault` or some other linking error. When a loader links with your program, it contains the symbols of all or some Vulkan functions. When your program will execute, the loader will search for a valid `ICD` on your system (eg : `/usr/lib/libvulkan_intel.so.` in linux for intel integrated graphics) . Once, a valid `ICD` is found, the loader loads it using the system libraries like `dl` on linux. It loads the file in the memory and then get's the memory address of functions using their names. Once we have the address, these addresses are stored in those symbols defined by the loader earlier. This way when you call the function `doSomething(xyz)`, your request is first recieved by the loader and then redirected to the `ICD`, or more precisely the graphics driver that the hardware vendor developed for you to talk to their hardware.

This way the loader allows you to use multiple `ICD`s at a time! You just need to have the driver and the `ICD` present on your system. Below is an image to show how loader works

![High Level View of Loader](https://github.com/KhronosGroup/Vulkan-Loader/raw/master/loader/images/high_level_loader.png)source : Khronos Group

### Vulkan Validation Layers

Layers are helper libraries that intercept your application's communication with the GPU (Vulkan Physical Device or `VkPhysicalDevice`). They help you in detecting bugs and errors in your program for easy debugging. They are an optional feature and hence can be removed anytime. This plugin type behaviour helps when you are deploying your game. In the debug builds, you can enable them to look for errors and in the release builds you can turn them off. This way, there won't be any error checking in release builds of your game and it will run faster as compared to the debug builds. In APIs like OpenGL, error checking is done all the time which adds an extra performance drop in the application. Layers sometimes also modify your function calls just in order to make them work.In the above image, you can see the connection between loader and the layer is a two way communication channel.

### Vulkan Extensions

Vulkan extensions are libraries that augment the Vulkan API by providing more platform specific or device specific feature. For example : Vulkan does not provide a direct way to display rendererd images onto the screen. For this there is a Vulkan extension for creating surface for the window created by application. This extension is named `VK_KHR_surface`. This extension can be used to display rendered images onto the window we create. This extension is used in combination with a platform specific extension for surface creation. There is an extension named `VK_KHR_swapchain` to create a swapchain for us. A swapchain is used to replace image currenly presented on screen with a newly rendered one.

### Vulkan Instance

A Vulkan Instance (`VkInstance`) represents our communication state with Vulkan. In OpenGL, the system is initialized for all application and all applications communicate with same instance of OpenGL but here in Vulkan, all applications talk to Vulkan in a different manner so they each need to have their own Vulkan Instance. This instance stores our application information and defines how we are going to use Vulkan. This `VkInstance` (and all vulkan handles) is actually a pointer to a data structure defined in the Vulkan loader