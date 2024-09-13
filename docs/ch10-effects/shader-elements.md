# Shader Elements（着色器元素）

For programming shaders, Qt Quick provides two elements. The `ShaderEffectSource` and the `ShaderEffect`. The shader effect applies custom shaders and the shader effect source renders a QML item into a texture and renders it. As shader effect can apply custom shaders to its rectangular shape and can use sources for the shader operation. A source can be an image, which is used as a texture or a shader effect source.

对于编程着色器，Qt Quick 提供了两个元素。`ShaderEffectSource` 和 `ShaderEffect`。ShaderEffect 应用自定义着色器，而 ShaderEffectSource 将 QML 项目渲染为纹理并渲染它。由于 ShaderEffect 可以对其矩形形状应用自定义着色器，并且可以使用源进行着色器操作，因此源可以是图像，用作纹理。

The default shader uses the source and renders it unmodified. Below, we first see the QML file with two `ShaderEffect` elements. One without any shaders specified, and one explicitly specifying default vertex and fragment shaders. We will look at the shaders shortly.

默认着色器使用源并对其进行未修改的渲染。下面，我们首先看到带有两个 `ShaderEffect` 元素的 QML 文件。一个没有指定任何着色器，另一个明确指定了默认顶点和片段着色器。我们很快就会看到这些着色器。

<<< @/docs/ch10-effects/src/effects/default/defaultshader.qml#M1

![image](./assets/defaultshader.png)

In the above example, we have a row of 3 images. The first is the real image. The second is rendered using the default shader and the third is rendered using the shader code for the fragment and vertex as shown below. Let's have a look at the shaders.

在上面的示例中，我们有一排 3 张图片。第一张是真实图片。第二张使用默认着色器渲染，第三张使用如下所示的片段和顶点着色器代码渲染。让我们来看看这些着色器。


The vertex shader takes the texture coordinate, `qt_MultiTexCoord0`, and propagates it to the `qt_TexCoord0` variable. It also takes the `qt_Vertex` position and multiplies it with Qt's transformation matrix, `ubuf.qt_Matrix`, and returns it through the `gl_Position` variable. This leaves the texture and vertex position on the screen unmodified.

顶点着色器接受纹理坐标 `qt_MultiTexCoord0` 并将其传播到 `qt_TexCoord0` 变量中。它还接受 `qt_Vertex` 位置并将其与 Qt 的变换矩阵 `ubuf.qt_Matrix` 相乘，并通过 `gl_Position` 变量返回。这使纹理和顶点位置在屏幕上保持不变。

<<< @/docs/ch10-effects/src/effects/default/default.vert#M1

The fragment shader takes the texture from the `source` 2D sampler, i.e. the texture, at the coordinate `qt_TexCoord0` and multiplies it with the Qt opacity, `ubuf.qt_Opacity` to calculate the `fragColor` which is the color to be used for the pixel.

片段着色器从 `source` 2D 池中获取纹理，即坐标 `qt_TexCoord0` 处的纹理，并将其与 Qt 不透明度 `ubuf.qt_Opacity` 相乘以计算 `fragColor`，这是用于像素的颜色。



<<< @/docs/ch10-effects/src/effects/default/default.frag#M1

Notice that these two shaders can serve as the boilerplate code for your own shaders. The variables, locations and bindings, are what Qt expects. You can read more about the exact details of this on the [Shader Effect Documentation](https://doc-snapshots.qt.io/qt6-6.2/qml-qtquick-shadereffect.html#details).

请注意，这两个着色器可以作为您自己的着色器的样板代码。变量、位置和绑定是 Qt 所期望的。您可以在 [Shader Effect 文档](https://doc-snapshots.qt.io/qt6-6.2/qml-qtquick-shadereffect.html#details) 中阅读更多关于此的详细信息。

Before we can use the shaders, they need to be baked. If the shaders are a part of a larger Qt project and included as resources, this can be automated. However, when working with the shaders and a `qml`-file, we need to explicitly bake them by hand. This is done using the following two commands:

在我们使用着色器之前，需要先烤制它们。如果着色器是更大 Qt 项目的一部分，并且作为资源包含在内，则可以自动化此操作。但是，当使用着色器和 `qml` 文件时，我们需要明确手动烤制它们。这是使用以下两个命令完成的：

```
qsb --glsl 100es,120,150 --hlsl 50 --msl 12    -o default.frag.qsb default.frag 
qsb --glsl 100es,120,150 --hlsl 50 --msl 12 -b -o default.vert.qsb default.vert
```

The `qsb` tool is located in the `bin` directory of your Qt 6 installation.

`qsb` 工具位于您的 Qt 6 安装目录中的 `bin` 目录中。


::: tip
If you don’t want to see the source image and only the effected image you can set the *Image* to invisible (\`\` visible: false\`\`). The shader effects will still use the image data just the *Image* element will not be rendered.

如果您不想看到源图像，而只想看到受影响的图像，可以将 *Image* 设置为不可见（\`\` visible: false\`\`）。着色器效果仍然会使用图像数据，只是 *Image* 元素不会渲染。
:::

In the next examples, we will be playing around with some simple shader mechanics. First, we concentrate on the fragment shader and then we will come back to the vertex shader.

在下一个示例中，我们将玩一些简单的着色器机制。首先，我们专注于片段着色器，然后我们将回到顶点着色器。
