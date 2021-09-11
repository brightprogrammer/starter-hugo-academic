---
title: Game Engine Series 0x0002
subtitle: Introduction To Vulkan
date: 2021-09-11T14:12:31.326Z
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
This post will be on introduction to Vulkan API. We will setup our initiialization code and make our new window. Let's begin!

### What is Vulkan ?

Vulkan is a fast, new and modern 3D graphics programming API. The initial release was about 5 years ago and it is now used my many games and game engines. The API was initially made by AMD for modern GPUs and was initially named Mantle which was later made open source, renamed to Vulkan and is now maintained by the **Khronos Group**. It is written purely in C and supports many platforms like : [Android](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Android&stick=H4sIAAAAAAAAAONgVuLSz9U3MCqvKEkvX8TK7piXUpSfmQIAyAsAJhgAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoAXoECEIQAw), [Linux](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Linux&stick=H4sIAAAAAAAAAONgVuLUz9U3SCuoqipYxMrqk5lXWgEATgerNhUAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoAnoECEIQBA), [Fuchsia](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Fuchsia&stick=H4sIAAAAAAAAAONgVuLVT9c3NEw2zEnLKKoqXsTK7laanFGcmQgAxklZLRsAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoA3oECEIQBQ), [BSD Unix](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=BSD+Unix&stick=H4sIAAAAAAAAAONgVuLQz9U3MEwzzVnEyuEU7KIQmpdZAQDAUa93FwAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoBHoECEIQBg), [QNX](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=QNX&stick=H4sIAAAAAAAAAONgVuLQz9U3yEhKSlnEyhzoFwEA4JwBWRIAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoBXoECEIQBw), [Windows](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Windows&stick=H4sIAAAAAAAAAONgVuLQz9U3MCmKt1jEyh6emZeSX14MAFTjqsQWAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoBnoECEIQCA), [Nintendo Switch](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Nintendo+Switch&stick=H4sIAAAAAAAAAONgVuLVT9c3NEw2K7FMMzPMWcTK75eZV5Kal5KvEFyeWZKcAQB0QcE-IwAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoB3oECEIQCQ), [Stadia](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Stadia&stick=H4sIAAAAAAAAAONgVuLVT9c3NEzLMs8oyUiPX8TKFlySmJKZCACloW6EGgAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoCHoECEIQCg), [Tizen](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Tizen&stick=H4sIAAAAAAAAAONgVuLSz9U3yDDPqchKWcTKGpJZlZoHADhg-LkWAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoCXoECEIQCw), [macOS](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=macOS&stick=H4sIAAAAAAAAAONgVuLQz9U3MDWtLFrEypqbmOwfDADb5b_yFAAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoCnoECEIQDA), [IOS](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=IOS&stick=H4sIAAAAAAAAAONgVuLSz9U3MC5PyjE0WcTK7OkfDAC-b2NaFAAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoC3oECEIQDQ), [Raspberry Pi](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Raspberry+Pi&stick=H4sIAAAAAAAAAONgVuLSz9U3SM9NNzZLX8TKE5RYXJCUWlRUqRCQCQA5myHgHQAAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoDHoECEIQDg). It uses [Apache License](https://www.google.com/search?client=firefox-b-d&sxsrf=AOaemvJ0adMSWlfVR9scr93Ko9YiffsEIA:1631340626919&q=Apache+License&stick=H4sIAAAAAAAAAONgVuLUz9U3MDTKjrdcxMrnWJCYnJGq4JOZnJpXnAoAzGCpBx4AAAA&sa=X&ved=2ahUKEwijnsy0ofbyAhXymeYKHYs3CygQmxMoAXoECEgQAw) 2.0 which means we can use it too.

### But why is it called an API?

Well, this is because it is an API. Vulkan API is a set of rules (functions with defined valid usage and invalid usage etc..) defined in the [vkspec](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html) which hardware vendors follow, more like make an implementation out of it to make their device driver. This driver contains functions with same names and arguments as defined in [vkspec](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html) and these functions, no matter how they are implemented in the inside must look and work in the same way as defined by the [vkspec](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html). The whole purpose of this API is to hide the actual implemetation from the user. This way no matter how the hardware vendors implement their device driver, we can use it as long as it follows the API rules. The functions defined in the device driver can be used to interact with GPU or any other device that can do some processing using the device driver provided by the hardware vendor. In simple words, Vulkan API is the header (.hpp) of a class and the `ICD` is the definition(.cpp) of that class and you are using this class in your application. So, you see Vulkan is combination of both the specification and the implementation (`ICD`) which makes the Vulkan API. First the API is decided, then a specification (vkspec) and implementation (drivers) is made for that version of API.

### Can't we just directly talk to GPU?

Short answer : **No**

Long answer : You cannot (you can in some cases like intel and amd), because that'd mean the GPU vendor revelaing their secrets to you. Each graphic card manufacturer try to make their product faster than their competetors and in that pursuit they research a lot and make their own methods to do stuffs faster and those methods are to be kept confidential. That is why we have a graphics driver. It is a platform and graphics card dependent library which contains the functions that [Vulkan Spec](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html) defines and we link to that library to interact with the GPU.

To get an idea of how Intel driver works see the [Anvil source code](https://gitlab.freedesktop.org/mesa/mesa/-/tree/main/src/intel/vulkan) and [RADV source code](https://gitlab.freedesktop.org/mesa/mesa/-/tree/main/src/amd/vulkan) for AMD.

### But, how exactly do we talk to GPU?

As stated above, we link our program with the graphics driver in order to make it talk to GPU. But wouldn't that mean that we have to have graphics driver for every vendor our there? No, you don't need to do that! This is where the interface comes in. Since, Vulkan is an interface between our program and GPU, as long as both the hardware vendor and the developer follow the rules defined by Vulkan API to use it's interface, then they don't need to care about each other. You can just assume that the system that your program will run in will already have the graphics drivers in place and then we can just link to it dynamically at runtime because ofcourse static linking will not be possible here. 

### How exactly will I link to driver dynamically?

You use the [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader) developed by the Khronos Group. You can link to this loader either dynamically (like what volk does or the normal way) or statically (what we will do). For help with the former method, refer to the following resources (if you want to make your own loader instead of using [volk](https://github.com/zeux/volk) or [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader)) : 

* For Linux or other Unix based systems : <https://tldp.org/HOWTO/Program-Library-HOWTO/dl-libraries.html>
* For Windows : <https://docs.microsoft.com/en-us/windows/win32/dlls/using-run-time-dynamic-linking>
* Intel tutorial on how to do it for vulkan : <https://software.intel.com/content/www/us/en/develop/articles/api-without-secrets-introduction-to-vulkan-part-1.html>

For the latter method, we just have to download [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader) and link to it statically or dynamically however you like. This is the method I will chose. There is a small performance gain if you use a custom loader instead, I, however at this moment just want to see something on my screen ASAP.

Volk (and similar implementations) get's it's perfomance boost because it skips the [trampoline](http://kylehalladay.com/blog/2020/11/13/Hooking-By-Example.html) (also known as hooking) part in Vulkan Loader. An example image retrieved from khronos site is :

![Webinar Recap: Vulkan Loader Deep Dive](https://www.khronos.org/assets/uploads/blogs/2017-Webinar-Recap-Vulkan-Loader-Deep-Dive.jpg)Here is a paragraph from a [vulkan.lunarg.com post](https://vulkan.lunarg.com/doc/view/1.2.170.0/windows/loader_and_layer_interface.html) : 

> ##### [](https://vulkan.lunarg.com/doc/view/1.2.170.0/windows/loader_and_layer_interface.html#user-content-indirectly-linking-to-the-loader)Indirectly Linking to the Loader
>
> Applications are not required to link directly to the loader library, instead they can use the appropriate platform-specific dynamic symbol lookup on the loader library to initialize the application's own dispatch table. This allows an application to fail gracefully if the loader cannot be found. It also provides the fastest mechanism for the application to call Vulkan functions. An application will only need to query (via system calls such as `dlsym`) the address of `vkGetInstanceProcAddr` from the loader library. Using `vkGetInstanceProcAddr` the application can then discover the address of all functions and extensions available, such as `vkCreateInstance`, `vkEnumerateInstanceExtensionProperties` and `vkEnumerateInstanceLayerProperties` in a platform-independent way.
>
> ##### Best Application Performance Setup
>
> If you desire the best performance possible, you should set up your own dispatch table so that all your Instance functions are queried using `vkGetInstanceProcAddr` and all your Device functions are queried using `vkGetDeviceProcAddr`.

So notice that when no layers are enabled, you'll be directly talking to the `ICD`.

### How does the loader work?

The main task of loader is to search for the graphics driver, also called `Installable Client Driver` (`ICD`) and then get pointers to all or some functions from the library file after loading it into memory. Say you are calling some function named `doSomething(xyz)`. Now, this function has to be defined somewhere otherwise your program will show a `segfault` or some other linking error. When a loader links with your program, it contains the symbols of all or some Vulkan functions. When your program will execute, the loader will search for a valid `ICD` on your system (eg : `/usr/lib/libvulkan_intel.so.` in linux for intel integrated graphics) . Once, a valid `ICD` is found, the loader loads it using the system libraries like `dl` on linux. It loads the file in the memory and then get's the memory address of functions using their names. Once we have the address, these addresses are stored in those symbols defined by the loader earlier. This way when you call the function `doSomething(xyz)`, your request is first recieved by the loader and then redirected to the `ICD`, or more precisely the graphics driver that the hardware vendor developed for you to talk to their hardware.

This way the loader allows you to use multiple `ICD`s at a time! You just need to have the driver and the `ICD` present on your system. Below is an image to show how loader works

![High Level View of Loader](https://github.com/KhronosGroup/Vulkan-Loader/raw/master/loader/images/high_level_loader.png)source : Khronos Group

In Linux you will generally find the loader as a shared object file named `/usr/lib/libvulkan.so`. You will see a library file named the same in our projects `/lib` directory because remember we build this as a dependency.

### Vulkan Validation Layers

Validation layers are helper libraries that intercept your application's communication with the GPU (Vulkan Physical Device or `VkPhysicalDevice`). They help you in detecting bugs and errors in your program for easy debugging. They are an optional feature and hence can be removed anytime. This plugin type behaviour helps when you are deploying your game. In the debug builds, you can enable them to look for errors and in the release builds you can turn them off. This way, there won't be any error checking in release builds of your game and it will run faster as compared to the debug builds. In APIs like OpenGL, error checking is done all the time which adds an extra performance drop in the application. Layers sometimes also modify your function calls just in order to make them work.In the above image, you can see the connection between loader and the layer is a two way communication channel.

### Vulkan Extensions

Vulkan extensions augment the Vulkan API by providing more platform specific or device specific feature. For example : Vulkan does not provide a direct way to display rendered images onto the screen. For this there is a Vulkan extension for creating a display surface for the window created by application. This extension is named `VK_KHR_surface`. This extension can be used to display rendered images onto the window we create. This extension is used in combination with a platform specific extension for surface creation. There is an extension named `VK_KHR_swapchain` to create a swapchain for us. A swapchain is used to replace image currenly presented on screen with a newly rendered one.

### Vulkan Instance ([`VkInstance`](https://vulkan.lunarg.com/doc/view/1.2.162.0/mac/1.2-extensions/vkspec.html#initialization-instances))

A Vulkan Instance ([`VkInstance`](https://vulkan.lunarg.com/doc/view/1.2.162.0/mac/1.2-extensions/vkspec.html#initialization-instances)) represents our communication state with Vulkan. In OpenGL, the system is initialized for all application and all applications communicate with same instance of OpenGL but here in Vulkan, all applications talk to Vulkan in a different manner so they each need to have their own Vulkan Instance. This instance stores our application information and defines how we are going to use Vulkan. Below is a statement from vkspec

> There is no global state in Vulkan and all per-application state is stored in a `VkInstance` object. Creating a `VkInstance` object initializes the Vulkan library and allows the application to pass information about itself to the implementation.

When we will create a [`VkInstance`](https://vulkan.lunarg.com/doc/view/1.2.162.0/mac/1.2-extensions/vkspec.html#initialization-instances), we will request instance level extensions and layers to be enabled. The loader will forward our calls or enabled layers and extensions when needed.

This `VkInstance` is actually a pointer to a data structure defined somewhere in the driver code. Here is an example how Anvil implements a Vulkan instance : 

```cpp
#include "vk_instance.h"

struct anv_instance {
    struct vk_instance                          vk;

    bool                                        physical_devices_enumerated;
    struct list_head                            physical_devices;

    bool                                        pipeline_cache_enabled;

    struct driOptionCache                       dri_options;
    struct driOptionCache                       available_dri_options;
};
```

What you will be able to use in your code is only a pointer to this data structure and some functions to manipulate this struct. You are only restricted to using the API and not this code.

Here is how RADV for AMD implements it : 

```cpp
#include "vk_instance.h"

struct radv_instance {
   struct vk_instance vk;

   VkAllocationCallbacks alloc;

   uint64_t debug_flags;
   uint64_t perftest_flags;

   bool physical_devices_enumerated;
   struct list_head physical_devices;

   struct driOptionCache dri_options;
   struct driOptionCache available_dri_options;

   /**
    * Workarounds for game bugs.
    */
   bool enable_mrt_output_nan_fixup;
   bool disable_tc_compat_htile_in_general;
   bool disable_shrink_image_store;
   bool absolute_depth_bias;
   bool report_apu_as_dgpu;
};
```

Notice the change in definition of instance when the device changes. Definiton of `vk_instance` can be found in `mesa/src/vulkan/util` to be  : 

```cpp
// in vk_objects.h
struct vk_object_base {
   VK_LOADER_DATA _loader_data;
   VkObjectType type;

   struct vk_device *device;

   /* For VK_EXT_private_data */
   struct util_sparse_array private_data;
};

// in vk_instance.h
struct vk_instance {
   struct vk_object_base base;
   VkAllocationCallbacks alloc;

   struct vk_app_info app_info;
   struct vk_instance_extension_table enabled_extensions;

   struct vk_instance_dispatch_table dispatch_table;

   /* VK_EXT_debug_report debug callbacks */
   struct {
      mtx_t callbacks_mutex;
      struct list_head callbacks;
   } debug_report;
};
```

There is a `struct` named [loader_instance](https://github.com/KhronosGroup/Vulkan-Loader/blob/e1dc222803390e8e18e4a9a211609341ea34ccab/loader/loader_common.h#L230) in the [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader) too but that is only a wrapper around these actual implementations.

```cpp
// Per instance structure
struct loader_instance {
    struct loader_instance_dispatch_table *disp;  // must be first entry in structure

    // Vulkan API version the app is intending to use.
    uint16_t app_api_major_version;
    uint16_t app_api_minor_version;

    // We need to manually track physical devices over time.  If the user
    // re-queries the information, we don't want to delete old data or
    // create new data unless necessary.
    uint32_t total_gpu_count;
    uint32_t phys_dev_count_term;
    struct loader_physical_device_term **phys_devs_term;
    uint32_t phys_dev_count_tramp;
    struct loader_physical_device_tramp **phys_devs_tramp;

    // We also need to manually track physical device groups, but we don't need
    // loader specific structures since we have that content in the physical
    // device stored internal to the public structures.
    uint32_t phys_dev_group_count_term;
    struct VkPhysicalDeviceGroupProperties **phys_dev_groups_term;
    uint32_t phys_dev_group_count_tramp;
    struct VkPhysicalDeviceGroupProperties **phys_dev_groups_tramp;

    struct loader_instance *next;

    uint32_t total_icd_count;
    struct loader_icd_term *icd_terms;
    struct loader_icd_tramp_list icd_tramp_list;

    struct loader_dispatch_hash_entry dev_ext_disp_hash[MAX_NUM_UNKNOWN_EXTS];
    struct loader_dispatch_hash_entry phys_dev_ext_disp_hash[MAX_NUM_UNKNOWN_EXTS];

    struct loader_msg_callback_map_entry *icd_msg_callback_map;

    struct loader_layer_list instance_layer_list;
    bool override_layer_present;

    // List of activated layers.
    //  app_      is the version based on exactly what the application asked for.
    //            This is what must be returned to the application on Enumerate calls.
    //  expanded_ is the version based on expanding meta-layers into their
    //            individual component layers.  This is what is used internally.
    struct loader_layer_list app_activated_layer_list;
    struct loader_layer_list expanded_activated_layer_list;

    VkInstance instance;  // layers/ICD instance returned to trampoline

    struct loader_extension_list ext_list;  // icds and loaders extensions
    union loader_instance_extension_enables enabled_known_extensions;

    VkLayerDbgFunctionNode *DbgFunctionHead;
    uint32_t num_tmp_report_callbacks;
    VkDebugReportCallbackCreateInfoEXT *tmp_report_create_infos;
    VkDebugReportCallbackEXT *tmp_report_callbacks;
    uint32_t num_tmp_messengers;
    VkDebugUtilsMessengerCreateInfoEXT *tmp_messenger_create_infos;
    VkDebugUtilsMessengerEXT *tmp_messengers;

    VkAllocationCallbacks alloc_callbacks;

    bool wsi_surface_enabled;
#ifdef VK_USE_PLATFORM_WIN32_KHR
    bool wsi_win32_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_WAYLAND_KHR
    bool wsi_wayland_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_XCB_KHR
    bool wsi_xcb_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_XLIB_KHR
    bool wsi_xlib_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_DIRECTFB_EXT
    bool wsi_directfb_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_ANDROID_KHR
    bool wsi_android_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_MACOS_MVK
    bool wsi_macos_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_IOS_MVK
    bool wsi_ios_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_GGP
    bool wsi_ggp_surface_enabled;
#endif
    bool wsi_headless_surface_enabled;
#if defined(VK_USE_PLATFORM_METAL_EXT)
    bool wsi_metal_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_FUCHSIA
    bool wsi_imagepipe_surface_enabled;
#endif
#ifdef VK_USE_PLATFORM_SCREEN_QNX
    bool wsi_screen_surface_enabled;
#endif
    bool wsi_display_enabled;
    bool wsi_display_props2_enabled;
};
```

`VkInstance` is actually a typedef to `VkInstance_T*` as you will see here : 

```
// link : https://github.com/KhronosGroup/Vulkan-Headers/blob/4fee3efc189c83ccd26a9cd8265185c98458c94d/include/vulkan/vulkan_core.h#L25
#define VK_DEFINE_HANDLE(object) typedef struct object##_T* object;

// link : https://github.com/KhronosGroup/Vulkan-Headers/blob/4fee3efc189c83ccd26a9cd8265185c98458c94d/include/vulkan/vulkan_core.h#L100
VK_DEFINE_HANDLE(VkInstance)

```



### Vulkan Physical Device (`VkPhysicalDevice`)  

A Physical Device represents any Vulkan compatible device connected to your system. By Vulkan compatible, I mean that there is an `ICD` present for it on your system. Since your GPU has a device driver that conforms to the Vulkan API, it's Vulkan compatible. If the loader detects an `ICD` present on your system, it'll create a struct that wraps the actual physical device struct in the `ICD` and return it's pointer to us. We can then use this to talk to our GPU in future. Below is an example from RADV to show how it is implemented in the driver : 

```cpp
struct radv_physical_device {
   struct vk_physical_device vk;

   /* Link in radv_instance::physical_devices */
   struct list_head link;

   struct radv_instance *instance;

   struct radeon_winsys *ws;
   struct radeon_info rad_info;
   char name[VK_MAX_PHYSICAL_DEVICE_NAME_SIZE];
   uint8_t driver_uuid[VK_UUID_SIZE];
   uint8_t device_uuid[VK_UUID_SIZE];
   uint8_t cache_uuid[VK_UUID_SIZE];

   int local_fd;
   int master_fd;
   struct wsi_device wsi_device;

   bool out_of_order_rast_allowed;

   /* Whether DCC should be enabled for MSAA textures. */
   bool dcc_msaa_allowed;

   /* Whether to enable NGG. */
   bool use_ngg;

   /* Whether to enable NGG streamout. */
   bool use_ngg_streamout;

   /* Number of threads per wave. */
   uint8_t ps_wave_size;
   uint8_t cs_wave_size;
   uint8_t ge_wave_size;

   /* Whether to use the LLVM compiler backend */
   bool use_llvm;

   /* This is the drivers on-disk cache used as a fallback as opposed to
    * the pipeline cache defined by apps.
    */
   struct disk_cache *disk_cache;

   VkPhysicalDeviceMemoryProperties memory_properties;
   enum radeon_bo_domain memory_domains[VK_MAX_MEMORY_TYPES];
   enum radeon_bo_flag memory_flags[VK_MAX_MEMORY_TYPES];
   unsigned heaps;

#ifndef _WIN32
   int available_nodes;
   drmPciBusInfo bus_info;

   dev_t primary_devid;
   dev_t render_devid;
#endif

   nir_shader_compiler_options nir_options;
};
```

and again this will be different from what Anvil implements : 

```cpp
struct anv_physical_device {
    struct vk_physical_device                   vk;

    /* Link in anv_instance::physical_devices */
    struct list_head                            link;

    struct anv_instance *                       instance;
    char                                        path[20];
    struct {
       uint16_t                                 domain;
       uint8_t                                  bus;
       uint8_t                                  device;
       uint8_t                                  function;
    }                                           pci_info;
    struct intel_device_info                      info;
    /** Amount of "GPU memory" we want to advertise
     *
     * Clearly, this value is bogus since Intel is a UMA architecture.  On
     * gfx7 platforms, we are limited by GTT size unless we want to implement
     * fine-grained tracking and GTT splitting.  On Broadwell and above we are
     * practically unlimited.  However, we will never report more than 3/4 of
     * the total system ram to try and avoid running out of RAM.
     */
    bool                                        supports_48bit_addresses;
    struct brw_compiler *                       compiler;
    struct isl_device                           isl_dev;
    struct intel_perf_config *                    perf;
    /*
     * Number of commands required to implement a performance query begin +
     * end.
     */
    uint32_t                                    n_perf_query_commands;
    int                                         cmd_parser_version;
    bool                                        has_exec_async;
    bool                                        has_exec_capture;
    bool                                        has_exec_fence;
    bool                                        has_syncobj_wait;
    bool                                        has_syncobj_wait_available;
    bool                                        has_context_priority;
    bool                                        has_context_isolation;
    bool                                        has_thread_submit;
    bool                                        has_mmap_offset;
    bool                                        has_userptr_probe;
    uint64_t                                    gtt_size;

    bool                                        use_softpin;
    bool                                        always_use_bindless;
    bool                                        use_call_secondary;

    /** True if we can access buffers using A64 messages */
    bool                                        has_a64_buffer_access;
    /** True if we can use bindless access for images */
    bool                                        has_bindless_images;
    /** True if we can use bindless access for samplers */
    bool                                        has_bindless_samplers;
    /** True if we can use timeline semaphores through execbuf */
    bool                                        has_exec_timeline;

    /** True if we can read the GPU timestamp register
     *
     * When running in a virtual context, the timestamp register is unreadable
     * on Gfx12+.
     */
    bool                                        has_reg_timestamp;

    /** True if this device has implicit AUX
     *
     * If true, CCS is handled as an implicit attachment to the BO rather than
     * as an explicitly bound surface.
     */
    bool                                        has_implicit_ccs;

    bool                                        always_flush_cache;

    uint32_t                                    subslice_total;

    struct {
      uint32_t                                  family_count;
      struct anv_queue_family                   families[ANV_MAX_QUEUE_FAMILIES];
    } queue;

    struct {
      uint32_t                                  type_count;
      struct anv_memory_type                    types[VK_MAX_MEMORY_TYPES];
      uint32_t                                  heap_count;
      struct anv_memory_heap                    heaps[VK_MAX_MEMORY_HEAPS];
      bool                                      need_clflush;
    } memory;

    struct anv_memregion                        vram;
    struct anv_memregion                        sys;
    uint8_t                                     driver_build_sha1[20];
    uint8_t                                     pipeline_cache_uuid[VK_UUID_SIZE];
    uint8_t                                     driver_uuid[VK_UUID_SIZE];
    uint8_t                                     device_uuid[VK_UUID_SIZE];

    struct disk_cache *                         disk_cache;

    struct wsi_device                       wsi_device;
    int                                         local_fd;
    bool                                        has_local;
    int64_t                                     local_major;
    int64_t                                     local_minor;
    int                                         master_fd;
    bool                                        has_master;
    int64_t                                     master_major;
    int64_t                                     master_minor;
    struct drm_i915_query_engine_info *         engine_info;

    void (*cmd_emit_timestamp)(struct anv_batch *, struct anv_bo *, uint32_t );
    struct intel_measure_device                 measure_device;
};
```

where `vk_physical_device` is : 

```cpp
// defined in mesa/vulkan/util/vk_physical_device.h   
struct vk_physical_device {
      struct vk_object_base base;
      struct vk_instance *instance;

      struct vk_device_extension_table supported_extensions;

      struct vk_physical_device_dispatch_table dispatch_table;
   };

```

When you ask Vulkan to get all physical devices present on system, it will look for a manifest file which provides the names and versions of layers that are present on the system. The location of this manifest file is provided in the environment variables of your system. It is generally located in `/usr/share/vulkan/explicit_layers.d`. A normal manifest file will look something like this :

```json
{
    "file_format_version": "1.2.0",
    "layer": {
        "name": "VK_LAYER_KHRONOS_validation",
        "type": "GLOBAL",
        "library_path": "libVkLayer_khronos_validation.so",
        "api_version": "1.2.184",
        "implementation_version": "1",
        "description": "Khronos Validation Layer",
        "introduction": "The main, comprehensive Khronos validation layer.\n\nVulkan is an Explicit API, enabling direct control over how GPUs actually work. By design, minimal error checking is done inside a Vulkan driver. Applications have full control and responsibility for correct operation. Any errors in how Vulkan is used can result in a crash. \n\nThe Khronos Valiation Layer can be enabled to assist development by enabling developers to verify their applications correctly use the Vulkan API.",
        "platforms": [ "WINDOWS", "LINUX", "ANDROID", "MACOS" ],
        "url": "https://vulkan.lunarg.com/doc/sdk/latest/windows/khronos_validation_layer.html",
        "instance_extensions": [
            {
                "name": "VK_EXT_debug_report",
                "spec_version": "9"
            },
            {
                "name": "VK_EXT_debug_utils",
                "spec_version": "1"
            },
            {
                "name": "VK_EXT_validation_features",
                "spec_version": "2"
            }
        ],
        "device_extensions": [
            {
                "name": "VK_EXT_debug_marker",
                "spec_version": "4",
                "entrypoints": [
                    "vkDebugMarkerSetObjectTagEXT",
                    "vkDebugMarkerSetObjectNameEXT",
                    "vkCmdDebugMarkerBeginEXT",
                    "vkCmdDebugMarkerEndEXT",
                    "vkCmdDebugMarkerInsertEXT"
                ]
            },
            {
                "name": "VK_EXT_validation_cache",
                "spec_version": "1",
                "entrypoints": [
                    "vkCreateValidationCacheEXT",
                    "vkDestroyValidationCacheEXT",
                    "vkGetValidationCacheDataEXT",
                    "vkMergeValidationCachesEXT"
                ]
            },
            {
                "name": "VK_EXT_tooling_info",
                "spec_version": "1",
                "entrypoints": [
                    "vkGetPhysicalDeviceToolPropertiesEXT"
                ]
            }
        ],
        "features": {
            "presets": [
                {
                    "label": "Standard",
                    "description": "Good default validation setup that balance validation coverage and performance.",
                    "platforms": [ "WINDOWS", "LINUX", "MACOS", "ANDROID" ],
                    "status": "STABLE",
                    "settings": [
                        {
                            "key": "enables",
                            "value": []
                        },
                        {
                            "key": "disables",
                            "value": [ "VK_VALIDATION_FEATURE_DISABLE_THREAD_SAFETY_EXT" ]
                        }
                    ]
                },
                {
                    "label": "Reduced-Overhead",
                    "description": "Disables some checks in the interest of better performance.",
                    "platforms": [ "WINDOWS", "LINUX", "MACOS" ],
                    "status": "STABLE",
                    "editor_state": "01110111111111111111111001111111111110",
                    "settings": [
                        {
                            "key": "enables",
                            "value": [
                            ]
                        },
                        {
                            "key": "disables",
                            "value": [
                                "VK_VALIDATION_FEATURE_DISABLE_THREAD_SAFETY_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_UNIQUE_HANDLES_EXT",
                                "VALIDATION_CHECK_DISABLE_IMAGE_LAYOUT_VALIDATION",
                                "VALIDATION_CHECK_DISABLE_COMMAND_BUFFER_STATE",
                                "VALIDATION_CHECK_DISABLE_OBJECT_IN_USE",
                                "VALIDATION_CHECK_DISABLE_QUERY_VALIDATION"
                            ]
                        }
                    ]
                },
                {
                    "label": "Best Practices",
                    "description": "Provides warnings about potential API misuse but valid usages of the API.",
                    "platforms": [ "WINDOWS", "LINUX", "MACOS", "ANDROID" ],
                    "status": "STABLE",
                    "settings": [
                        {
                            "key": "enables",
                            "value": [
                                "VK_VALIDATION_FEATURE_ENABLE_BEST_PRACTICES_EXT"
                            ]
                        },
                        {
                            "key": "disables",
                            "value": [
                                "VK_VALIDATION_FEATURE_DISABLE_THREAD_SAFETY_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_UNIQUE_HANDLES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_OBJECT_LIFETIMES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_API_PARAMETERS_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_CORE_CHECKS_EXT"
                            ]
                        }
                    ]
                },
                {
                    "label": "Synchronization",
                    "description": "Identify resource access conflicts due to missing or incorrect synchronization operations between actions reading or writing the same regions of memory.",
                    "platforms": [ "WINDOWS", "LINUX", "MACOS", "ANDROID" ],
                    "status": "STABLE",
                    "settings": [
                        {
                            "key": "enables",
                            "value": [
                                "VK_VALIDATION_FEATURE_ENABLE_SYNCHRONIZATION_VALIDATION_EXT"
                            ]
                        },
                        {
                            "key": "disables",
                            "value": [
                                "VK_VALIDATION_FEATURE_DISABLE_UNIQUE_HANDLES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_OBJECT_LIFETIMES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_API_PARAMETERS_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_CORE_CHECKS_EXT"
                            ]
                        }
                    ]
                },
                {
                    "label": "GPU-Assisted",
                    "description": "Check for API usage errors at shader execution time.",
                    "platforms": [ "WINDOWS", "LINUX" ],
                    "status": "STABLE",
                    "settings": [
                        {
                            "key": "enables",
                            "value": [
                                "VK_VALIDATION_FEATURE_ENABLE_GPU_ASSISTED_EXT",
                                "VK_VALIDATION_FEATURE_ENABLE_GPU_ASSISTED_RESERVE_BINDING_SLOT_EXT"
                            ]
                        },
                        {
                            "key": "disables",
                            "value": [
                                "VK_VALIDATION_FEATURE_DISABLE_THREAD_SAFETY_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_UNIQUE_HANDLES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_OBJECT_LIFETIMES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_API_PARAMETERS_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_CORE_CHECKS_EXT"
                            ]
                        }
                    ]
                },
                {
                    "label": "Shader Printf",
                    "description": "Debug shader code by \"printing\" any values of interest to the debug callback or stdout.",
                    "platforms": [ "WINDOWS", "LINUX" ],
                    "status": "STABLE",
                    "settings": [
                        {
                            "key": "enables",
                            "value": [
                                "VK_VALIDATION_FEATURE_ENABLE_DEBUG_PRINTF_EXT"
                            ]
                        },
                        {
                            "key": "disables",
                            "value": [
                                "VK_VALIDATION_FEATURE_DISABLE_THREAD_SAFETY_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_UNIQUE_HANDLES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_OBJECT_LIFETIMES_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_API_PARAMETERS_EXT",
                                "VK_VALIDATION_FEATURE_DISABLE_CORE_CHECKS_EXT"
                            ]
                        }
                    ]
                }
            ],
            "settings": [
                {
                    "key": "debug_action",
                    "label": "Debug Action",
                    "description": "This indicates what action is to be taken when a layer wants to report information",
                    "type": "FLAGS",
                    "flags": [
                        {
                            "key": "VK_DBG_LAYER_ACTION_LOG_MSG",
                            "label": "Log Message",
                            "description": "Log a txt message to stdout or to a log filename.",
                            "settings": [
                                {
                                    "key": "log_filename",
                                    "label": "Log Filename",
                                    "description": "Specifies the output filename",
                                    "type": "SAVE_FILE",
                                    "default": "stdout",
                                    "dependence": {
                                        "mode": "ALL",
                                        "settings": [
                                            {
                                                "key": "debug_action",
                                                "value": [ "VK_DBG_LAYER_ACTION_LOG_MSG" ]
                                            }
                                        ]
                                    }
                                }
                            ]
                        },
                        {
                            "key": "VK_DBG_LAYER_ACTION_CALLBACK",
                            "label": "Callback",
                            "description": "Call user defined callback function(s) that have been registered via the VK_EXT_debug_report extension. Since application must register callback, this is a NOOP for the settings file.",
                            "view": "HIDDEN"
                        },
                        {
                            "key": "VK_DBG_LAYER_ACTION_DEBUG_OUTPUT",
                            "label": "Debug Output",
                            "description": "Log a txt message using the Windows OutputDebugString function.",
                            "platforms": [ "WINDOWS"]
                        },
                        {
                            "key": "VK_DBG_LAYER_ACTION_BREAK",
                            "label": "Break",
                            "description": "Trigger a breakpoint if a debugger is in use."
                        }
                    ],
                    "default": [ "VK_DBG_LAYER_ACTION_LOG_MSG" ]
                },
                {
                    "key": "report_flags",
                    "label": "Message Severity",
                    "description": "This is a comma-delineated list of options telling the layer what types of messages it should report back",
                    "type": "FLAGS",
                    "flags": [
                        {
                            "key": "info",
                            "label": "Info",
                            "description": "Report informational messages."
                        },
                        {
                            "key": "warn",
                            "label": "Warning",
                            "description": "Report warnings from using the API in a manner which may lead to undefined behavior or to warn the user of common trouble spots. A warning does NOT necessarily signify illegal application behavior."
                        },
                        {
                            "key": "perf",
                            "label": "Performance",
                            "description": "Report using the API in a way that may cause suboptimal performance."
                        },
                        {
                            "key": "error",
                            "label": "Error",
                            "description": "Report errors in API usage."
                        },
                        {
                            "key": "debug",
                            "label": "Debug",
                            "description": "For layer development. Report messages for debugging layer behavior.",
                            "view": "HIDDEN"
                        }
                    ],
                    "default": [
                        "error",
                        "warn",
                        "perf"
                    ]
                },
                {
                    "key": "duplicate_message_limit",
                    "label": "Duplicated Messages Limit",
                    "description": "Limit the number of times any single validation message would be reported. 0 is unlimited.",
                    "type": "INT",
                    "env": "VK_LAYER_DUPLICATE_MESSAGE_LIMIT",
                    "default": 10,
                    "range": {
                        "min": 0
                    }
                },
                {
                    "key": "message_id_filter",
                    "label": "Mute Message VUIDs",
                    "description": "List of VUIDs and VUID identifers which are to be IGNORED by the validation layer",
                    "type": "LIST",
                    "env": "VK_LAYER_MESSAGE_ID_FILTER",
                    "default": []
                },
                {
                    "key": "disables",
                    "label": "Disables",
                    "description": "Setting an option here will disable areas of validation",
                    "type": "FLAGS",
                    "env": "VK_LAYER_DISABLES",
                    "flags": [
                        {
                            "key": "VK_VALIDATION_FEATURE_DISABLE_THREAD_SAFETY_EXT",
                            "label": "Thread Safety",
                            "description": "Thread checks. It may help with performance to run with thread-checking disabled most of the time, enabling it occasionally for a quick sanity check, or when debugging difficult application behaviors."
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_DISABLE_API_PARAMETERS_EXT",
                            "label": "Stateless Parameter",
                            "description": "Stateless parameter checks. This may not always be necessary late in a development cycle."
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_DISABLE_OBJECT_LIFETIMES_EXT",
                            "label": "Object Lifetime",
                            "description": "Object tracking checks. This may not always be necessary late in a development cycle."
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_DISABLE_CORE_CHECKS_EXT",
                            "label": "Core",
                            "description": "The main, heavy-duty validation checks. This may be valuable early in the development cycle to reduce validation output while correcting parameter/object usage errors."
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_DISABLE_UNIQUE_HANDLES_EXT",
                            "label": "Handle Wrapping",
                            "description": "Handle wrapping checks. Disable this feature if you are running into crashes when authoring new extensions or developing new Vulkan objects/structures"
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_DISABLE_SHADERS_EXT",
                            "label": "Shader Validation",
                            "description": "Shader checks. These checks can be CPU intensive during application start up, especially if Shader Validation Caching is also disabled.",
                            "view": "ADVANCED"
                        },
                        {
                            "key": "VALIDATION_CHECK_DISABLE_COMMAND_BUFFER_STATE",
                            "label": "Command Buffer State",
                            "description": "Check that all Vulkan objects used by a command buffer have not been destroyed. These checks can be CPU intensive for some applications.",
                            "view": "ADVANCED"
                        },
                        {
                            "key": "VALIDATION_CHECK_DISABLE_IMAGE_LAYOUT_VALIDATION",
                            "label": "Image Layout",
                            "description": "Check that the layout of each image subresource is correct whenever it is used by a command buffer. These checks are very CPU intensive for some applications.",
                            "view": "ADVANCED"
                        },
                        {
                            "key": "VALIDATION_CHECK_DISABLE_QUERY_VALIDATION",
                            "label": "Query",
                            "description": "Checks for commands that use VkQueryPool objects.",
                            "view": "ADVANCED"
                        },
                        {
                            "key": "VALIDATION_CHECK_DISABLE_OBJECT_IN_USE",
                            "label": "Object in Use",
                            "description": "Check that Vulkan objects are not in use by a command buffer when they are destroyed.",
                            "view": "ADVANCED"
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_DISABLE_SHADER_VALIDATION_CACHE_EXT",
                            "label": "Shader Validation Caching",
                            "description": "Disable caching of shader validation results",
                            "view": "ADVANCED"
                        }
                    ],
                    "default": [ "VK_VALIDATION_FEATURE_DISABLE_THREAD_SAFETY_EXT" ]
                },
                {
                    "key": "enables",
                    "label": "Enables",
                    "description": "Setting an option here will enable specialized areas of validation",
                    "type": "FLAGS",
                    "env": "VK_LAYER_ENABLES",
                    "flags": [
                        {
                            "key": "VK_VALIDATION_FEATURE_ENABLE_SYNCHRONIZATION_VALIDATION_EXT",
                            "label": "Synchronization",
                            "description": "This feature reports resource access conflicts due to missing or incorrect synchronization operations between actions (Draw, Copy, Dispatch, Blit) reading or writing the same regions of memory.",
                            "url": "${LUNARG_SDK}/synchronization_usage.html",
                            "status": "STABLE",
                            "platforms": [ "WINDOWS", "LINUX", "MACOS", "ANDROID" ]
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_ENABLE_DEBUG_PRINTF_EXT",
                            "label": "Debug Printf",
                            "description": "Enables processing of debug printf instructions in shaders and sending debug strings to the debug callback",
                            "url": "${LUNARG_SDK}/debug_printf.html",
                            "status": "STABLE",
                            "platforms": [ "WINDOWS", "LINUX" ],
                            "settings": [
                                {
                                    "key": "printf_to_stdout",
                                    "label": "Printf to stdout",
                                    "description": "To redirect Debug Printf messages from the debug callback to stdout",
                                    "type": "BOOL",
                                    "default": true,
                                    "platforms": [ "WINDOWS", "LINUX" ],
                                    "dependence": {
                                        "mode": "ANY",
                                        "settings": [
                                            {
                                                "key": "enables",
                                                "value": [ "VK_VALIDATION_FEATURE_ENABLE_DEBUG_PRINTF_EXT" ]
                                            }
                                        ]
                                    }
                                },
                                {
                                    "key": "printf_verbose",
                                    "label": "Printf verbose",
                                    "description": "Verbosity of debug printf messages",
                                    "type": "BOOL",
                                    "default": false,
                                    "platforms": [ "WINDOWS", "LINUX" ],
                                    "dependence": {
                                        "mode": "ANY",
                                        "settings": [
                                            {
                                                "key": "enables",
                                                "value": [ "VK_VALIDATION_FEATURE_ENABLE_DEBUG_PRINTF_EXT" ]
                                            }
                                        ]
                                    }
                                },
                                {
                                    "key": "printf_buffer_size",
                                    "label": "Printf buffer size",
                                    "description": "The size in bytes of the buffer used by debug printf",
                                    "type": "INT",
                                    "default": 1024,
                                    "range": {
                                        "min": 128,
                                        "max": 1048576
                                    },
                                    "unit": "bytes",
                                    "platforms": [ "WINDOWS", "LINUX" ],
                                    "dependence": {
                                        "mode": "ANY",
                                        "settings": [
                                            {
                                                "key": "enables",
                                                "value": [ "VK_VALIDATION_FEATURE_ENABLE_DEBUG_PRINTF_EXT" ]
                                            }
                                        ]
                                    }
                                }
                            ]
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_ENABLE_GPU_ASSISTED_EXT",
                            "label": "GPU-Assisted",
                            "description": "Check for API usage errors at shader execution time",
                            "url": "${LUNARG_SDK}/gpu_validation.html",
                            "platforms": [ "WINDOWS", "LINUX" ],
                            "settings": [
                                {
                                    "key": "gpuav_buffer_oob",
                                    "label": "Check Out of Bounds ",
                                    "description": "Enable buffer out of bounds checking",
                                    "type": "BOOL",
                                    "default": true,
                                    "platforms": [ "WINDOWS", "LINUX" ],
                                    "dependence": {
                                        "mode": "ANY",
                                        "settings": [
                                            {
                                                "key": "enables",
                                                "value": [ "VK_VALIDATION_FEATURE_ENABLE_GPU_ASSISTED_EXT" ]
                                            }
                                        ]
                                    }
                                },
                                {
                                    "key": "validate_draw_indirect",
                                    "label": "Check Draw Indirect Count Buffers and firstInstance values",
                                    "description": "Enable draw indirect checking",
                                    "type": "BOOL",
                                    "default": true,
                                    "platforms": [ "WINDOWS", "LINUX" ],
                                    "dependence": {
                                        "mode": "ANY",
                                        "settings": [
                                            {
                                                "key": "enables",
                                                "value": [ "VK_VALIDATION_FEATURE_ENABLE_GPU_ASSISTED_EXT" ]
                                            }
                                        ]
                                    }
                                }
                            ]
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_ENABLE_GPU_ASSISTED_RESERVE_BINDING_SLOT_EXT",
                            "label": "Reserve Descriptor Set Binding Slot",
                            "description": "Specifies that the validation layers reserve a descriptor set binding slot for their own use. The layer reports a value for VkPhysicalDeviceLimits::maxBoundDescriptorSets that is one less than the value reported by the device. If the device supports the binding of only one descriptor set, the validation layer does not perform GPU-assisted validation.",
                            "platforms": [ "WINDOWS", "LINUX" ]
                        },
                        {
                            "key": "VK_VALIDATION_FEATURE_ENABLE_BEST_PRACTICES_EXT",
                            "label": "Best Practices",
                            "description": "Activating this feature enables the output of warnings related to common misuse of the API, but which are not explicitly prohibited by the specification.",
                            "url": "${LUNARG_SDK}/best_practices.html",
                            "platforms": [ "WINDOWS", "LINUX", "MACOS", "ANDROID" ]
                        },
                        {
                            "key": "VALIDATION_CHECK_ENABLE_VENDOR_SPECIFIC_ARM",
                            "label": "ARM-specific best practices",
                            "description": "Enables processing of debug printf instructions in shaders and sending debug strings to the debug callback",
                            "platforms": [ "WINDOWS", "LINUX", "MACOS", "ANDROID" ]
                        }
                    ],
                    "default": []
                }
            ]
        }
    }
}

```

You can see names of all layers and extensions installed in your system by doing : 

```shellsession
➜  ~ vulkaninfo --summary
MESA-INTEL: warning: Performance support disabled, consider sysctl dev.i915.perf_stream_paranoid=0

==========
VULKANINFO
==========

Vulkan Instance Version: 1.2.187


Instance Extensions: count = 19
-------------------------------
VK_EXT_acquire_drm_display             : extension revision 1
VK_EXT_acquire_xlib_display            : extension revision 1
VK_EXT_debug_report                    : extension revision 10
VK_EXT_debug_utils                     : extension revision 2
VK_EXT_direct_mode_display             : extension revision 1
VK_EXT_display_surface_counter         : extension revision 1
VK_KHR_device_group_creation           : extension revision 1
VK_KHR_display                         : extension revision 23
VK_KHR_external_fence_capabilities     : extension revision 1
VK_KHR_external_memory_capabilities    : extension revision 1
VK_KHR_external_semaphore_capabilities : extension revision 1
VK_KHR_get_display_properties2         : extension revision 1
VK_KHR_get_physical_device_properties2 : extension revision 2
VK_KHR_get_surface_capabilities2       : extension revision 1
VK_KHR_surface                         : extension revision 25
VK_KHR_surface_protected_capabilities  : extension revision 1
VK_KHR_wayland_surface                 : extension revision 6
VK_KHR_xcb_surface                     : extension revision 6
VK_KHR_xlib_surface                    : extension revision 6

Instance Layers: count = 5
--------------------------
VK_LAYER_KHRONOS_validation       Khronos Validation Layer       1.2.184  version 1
VK_LAYER_LUNARG_api_dump          LunarG API dump layer          1.2.182  version 2
VK_LAYER_LUNARG_device_simulation LunarG device simulation layer 1.2.182  version 1
VK_LAYER_LUNARG_monitor           Execution Monitoring Layer     1.2.182  version 1
VK_LAYER_LUNARG_screenshot        LunarG image capture layer     1.2.182  version 1

Devices:
========
GPU0:
        apiVersion         = 4202678 (1.2.182)
        driverVersion      = 88088577 (0x5402001)
        vendorID           = 0x8086
        deviceID           = 0x5916
        deviceType         = PHYSICAL_DEVICE_TYPE_INTEGRATED_GPU
        deviceName         = Intel(R) HD Graphics 620 (KBL GT2)
        driverID           = DRIVER_ID_INTEL_OPEN_SOURCE_MESA
        driverName         = Intel open-source Mesa driver
        driverInfo         = Mesa 21.2.1
        conformanceVersion = 1.2.0.0
➜  ~ 
```

This will only work if you have a working installation of Vulkan. The best way to do this is by your system's package manager.

Note that I'm just providing you this code snippets in order to get an idea of what is being done on the inside. You mustn't think that this is how all hardware vendors do it. When I started learning Vulkan, I just had this itching sensation to know what exactly does this instance or physical device represent in code? You can easily skip these codes as we won't be using them anywhere but understanding this won't harm you. Instead this will help you when you read some low level implementation code like this.