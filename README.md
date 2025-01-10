![logo](http://nvidianews.nvidia.com/_ir/219/20157/NV_Designworks_logo_horizontal_greenblack.png)

# NVIDIA Vulkan Ray Tracing Tutorials

**This project** was created as a fork of the original **[nvpro-samples](https://github.com/nvpro-samples)** repository. The main goal is to explore and extend the capabilities of **real-time ray tracing (RTX)** in **Vulkan**, following the step-by-step tutorials and examples to gain a better understanding of its functionality. Additionally, modifications have been made to customize and improve some aspects of the original implementation and adapt it to this specific project.

In this case, the objective was to perform a **fluid physics simulation** using the **SPH (Smoothed Particle Hydrodynamics)** algorithm and render its graphical representation using **Vulkan** as the **API**. The simulation's physics calculations are performed in **CUDA** to optimize the computation, and the results are sent to **Vulkan** to update the screen. Work is still in progress to enable direct communication between **Vulkan** and **CUDA** without relying on the **PCI Express bus**, as there is a constant data flow between **GPU (CUDA) --> CPU --> GPU (Vulkan)**.

To run the project, you should follow the steps described in the original repository in the **[Setup](docs/setup.md)** section. Once this is done and the project is opened in **Visual Studio**, right-click on the **vk_ray_tracing__before_KHR** folder (where the entire project without external dependencies is located) and select **Properties**. Navigate to the "**CUDA C/C++"/"Command Line"" directory. With the **Debug** configuration selected, set the **"Additional Options"** command to **%(AdditionalOptions) -std=c++17 -Xcompiler="/EHsc -Ob2"**. With the **Release** configuration selected, set the **"Additional Options"** command to **%(AdditionalOptions) -std=c++17 -Xcompiler="/EHsc -Ob2"**.

## Results

These are the final results of the project, visualizing a simulation with 20,000 water particles:  
![RTXFluid](docs/Images/RTXFluid.png)  
![RTXWaterReflexes](docs/Images/RTXWaterReflexes.png)



---

![resultRaytraceShadowMedieval](docs/Images/resultRaytraceShadowMedieval.png)


The focus of this repository and the provided code is to showcase a basic integration of
[`ray tracing`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html#ray-tracing) and [`ray traversal`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html#ray-traversal) within an existing Vulkan sample, using the
[`VK_KHR_acceleration_structure`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html#VK_KHR_acceleration_structure), [`VK_KHR_ray_tracing_pipeline`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html#VK_KHR_ray_tracing_pipeline) and [`VK_KHR_ray_query`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html#VK_KHR_ray_query) extensions.

## Setup

To be able to compile and run those examples, please follow the [setup](docs/setup.md) instructions. Find more over nvpro-samples setup at: https://github.com/nvpro-samples/build_all.

## Tutorials 

The [first tutorial](https://nvpro-samples.github.io/vk_raytracing_tutorial_KHR/vkrt_tutorial.md.html) starts from a very simple Vulkan application. It loads a OBJ file and uses the rasterizer to render it. The tutorial then adds, **step-by-step**, all that is needed to be able to ray trace the scene.

-------
### Ray Tracing Tutorial: :arrow_forward: **[Start Here](https://nvpro-samples.github.io/vk_raytracing_tutorial_KHR/vkrt_tutorial.md.html)** :arrow_backward:

-------


## Extra Tutorials

All other tutorials start from the end of the _first_ ray tracing tutorial and also provide step-by-step instructions to modify and add methods and functions for that extra section.



Tutorial | Details
---------|--------
![small](docs/Images/anyhit.png) | [Any Hit Shader](ray_tracing_anyhit)<br>Implements transparent materials by adding a new shader to the Hit group and using the material information to discard hits over time. Adds an anyhit (.ahit) shader to the ray tracing pipeline. Creates simple transparency by randomly letting the ray hit or not.
![small](docs/Images/antialiasing.png) | [Jitter Camera](ray_tracing_jitter_cam)<br>  Anti-aliases the image by accumulating small variations of rays over time. Generates random ray directions. Read/write/accumulates the final image.
![img](docs/Images/instances.png) | [Thousands of Objects](ray_tracing_instances) <br> The current example allocates memory for each object, each of which has several buffers. This shows how to get around Vulkan's limits on the total number of memory allocations by using a memory allocator. Extends the limit of 4096 memory allocations. Uses these memory allocators: DMA, VMA.
![img](docs/Images/reflections.png) | [Reflections](ray_tracing_reflections) <br> Reflections can be implemented by shooting new rays from the closest hit shader, or by iteratively shooting them from the raygen shader. This example shows the limitations and differences of these implementations. Calls traceRayEXT() from the closest hit shader (recursive). Adds more data to the ray payload to continue the ray from the raygen shader.
![img](docs/Images/manyhits.png) | [Multiple Closest Hits Shader and Shader Records](ray_tracing_manyhits) <br> Explains how to add more closest hit shaders, choose which instance uses which shader, add data per SBT that can be retrieved in the shader, and more. One closest hit shader per object. Sharing closest hit shaders for some objects. Passing a shader record to the closest hit shader.
![img](docs/Images/animation2.gif) | [Animation](ray_tracing_animation) <br> This tutorial shows how animating the transformation matrices of the instances (TLAS) and animating the vertices of an object (BLAS) in a compute shader could be done. Refitting top level acceleration structures. Refitting bottom level acceleration structures.
![img](docs/Images/intersection.png) | [Intersection Shader](ray_tracing_intersection) <br> Adds thousands of implicit primitives and uses an intersection shader to render spheres and cubes. Explains what is needed to get procedural hit group working. Intersection Shaders. Sphere intersection. Axis aligned bounding box intersection.
![img](docs/Images/callable.png) | [Callable Shader](ray_tracing_callable) <br> Replacing if/else by callable shaders. The code to execute the lighting is done in separate callable shaders instead of being part of the main code. Adding multiple callable shaders. Calling ExecuteCallableEXT from the closest hit shader.
![img](docs/Images/rayquery.png) | [Ray Query](ray_tracing_rayquery) <br> Invokes ray intersection queries directly from the fragment shader to cast shadow rays. Ray tracing directly from the fragment shader.
![img](docs/Images/vk_ray_tracing_gltf_KHR_2.png) | [glTF Scene](ray_tracing_gltf) <br> Instead of loading separate OBJ objects, the example was modified to load glTF scene files containing multiple objects. This example is not about shading, but using more complex data than OBJ. However, it also shows a basic path tracer implementation.
![img](docs/Images/ray_tracing__advance.png) | [Advance](ray_tracing__advance) <br> An example combining most of the above samples in a single application.
![img](docs/Images/indirect_scissor/intro.png) | [Trace Rays Indirect](ray_tracing_indirect_scissor) <br> Teaches the use of `vkCmdTraceRaysIndirectKHR`, which sources width/height/depth from a buffer. As a use case, we add lanterns to the scene and use a compute shader to calculate scissor rectangles for each of them.
![img](docs/Images/ray_tracing_ao.png) | [AO Raytracing](ray_tracing_ao) <br> This extension to the tutorial is showing how G-Buffers from the fragment shader, can be used in a compute shader to cast ambient occlusion rays using ray queries ([GLSL_EXT_ray_query](https://github.com/KhronosGroup/GLSL/blob/master/extensions/ext/GLSL_EXT_ray_query.txt)).
![img](docs/Images/specialization.png) | [Specialization Constants](ray_tracing_specialization) <br> Showing how to use specialization constant and using interactively different specialization.
![img](docs/Images/high_level_advanced_compilation.png) | [Advanced Compilation](ray_tracing_advanced_compilation) <br> Shows how to create reusable pipeline libraries and compile pipelines on multiple threads.
![img](docs/Images/motionblur.png) | [Motion Blur](ray_tracing_motionblur) <br> Using vertex motion and instance motion: matrix and SRT.