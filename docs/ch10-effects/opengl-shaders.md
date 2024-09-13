# Graphics Shaders（图形着色器）

Graphics is rendered using a _rendering pipeline_ split into stages. There are multiple APIs to control graphics rendering. Qt supports OpenGL, Metal, Vulcan, and Direct3D. Looking at a simplified OpenGL pipeline, we can spot a vertex and fragment shader. These concepts exist for all other rendering pipelines too.

图形渲染使用一个_渲染管线_，分为多个阶段。有多个API来控制图形渲染。Qt支持OpenGL、Metal、Vulkan和Direct3D。查看简化的OpenGL管线，我们可以看到顶点着色器和片段着色器。这些概念在其他渲染管线中也存在。

![image](./assets/openglpipeline.png)

In the pipeline, the vertex shader receives vertex data, i.e. the location of the corners of each element that makes up the scene, and calculates a `gl_Position`. This means that the vertex shader can _move_ graphical elements. In the next stage, the vertexes are clipped, transformed and rasterized for pixel output. Then the pixels, also known as _fragments_,are passed through the fragment shader, which calculates the color of each pixel. The resulting color returned through the `gl_FragColor` variable. 

在管线中，顶点着色器接收顶点数据，即构成场景的每个元素角点的位置，并计算`gl_Position`。这意味着顶点着色器可以_移动_图形元素。在下一个阶段，顶点被裁剪、变换并光栅化以进行像素输出。然后，像素（也称为_片段_）通过片段着色器传递，该着色器计算每个像素的颜色。通过`gl_FragColor`变量返回的结果颜色。

To summarize: the vertex shader is called for each corner point of your polygon (vertex = point in 3D) and is responsible for any 3D manipulation of these points. The fragment (fragment = pixel) shader is called for each pixel and determines the color of that pixel.

总结：顶点着色器为多边形的每个角点（顶点=3D中的点）调用，并负责对这些点的任何3D操作。片段（片段=像素）着色器为每个像素调用，并确定该像素的颜色。


As Qt is independent of the underlying rendering API, Qt relies on a standard language for writing shaders. The Qt Shader Tools rely on a _Vulcan-compatible GLSL_. We will look more at this in the examples in this chapter.

由于Qt独立于底层渲染API，Qt依赖于一种标准语言来编写着色器。Qt着色器工具依赖于_Vulcan兼容的GLSL_。我们将在本章的示例中更详细地了解这一点。
