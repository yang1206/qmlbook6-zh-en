# Fragment Shaders(片段着色器)

The fragment shader is called for every pixel to be rendered. In this chapter, we will develop a small red lens which will increase the red color channel value of the source.

片段着色器为每个要渲染的像素调用一次。在本章中，我们将开发一个小红镜片，它将增加源图像的红色通道值。


## Setting up the scene(设置场景)

First, we set up our scene, with a grid centered in the field and our source image be displayed.

首先，我们设置场景，将网格居中显示，并将源图像显示出来。


<<< @/docs/ch10-effects/src/effects/redlense/1/redlense1.qml#M1

![image](./assets/redlense1.png)

## A red shader(红色着色器)

Next, we will add a shader, which displays a red rectangle by providing for each fragment a red color value.

接下来，我们将添加一个着色器，通过为每个片段提供一个红色颜色值来显示一个红色矩形。

<<< @/docs/ch10-effects/src/effects/redlense/2/red1.frag#M1{14-16}

In the fragment shader we simply assign a `vec4(1.0, 0.0, 0.0, 1.0)`, representing the color red with full opacity (alpha=1.0), to the `fragColor` for each fragment, turning each pixel to a solid red.

在片段着色器中，我们简单地将 `vec4(1.0, 0.0, 0.0, 1.0)` 分配给每个片段的 `fragColor`，表示红色，完全不透明（alpha=1.0），将每个像素变成纯红色。

![image](./assets/redlense2.png)

## A red shader with texture(带有纹理的红色着色器)

Now we want to apply the red color to each texture pixel. For this, we need the texture back in the vertex shader. As we don’t do anything else in the vertex shader the default vertex shader is enough for us. We just need to provide a compatible fragment shader.

现在我们想将红色应用到每个纹理像素上。为此，我们需要在顶点着色器中提供纹理。由于我们在顶点着色器中不做其他事情，因此默认的顶点着色器就足够了。我们只需要提供一个兼容的片段着色器。

<<< @/docs/ch10-effects/src/effects/redlense/2/red2.frag#M1{14-16}

The full shader contains now back our image source as variant property and we have left out the vertex shader, which if not specified is the default vertex shader.

完整的着色器现在包含我们的图像源作为可变属性，并且我们省略了顶点着色器，如果没有指定，则使用默认的顶点着色器。


In the fragment shader, we pick the texture fragment `texture(source, qt_TexCoord0)` and apply the red color to it.

在片段着色器中，我们选择纹理片段 `texture(source, qt_TexCoord0)` 并将其应用到红色上。


![image](./assets/redlense3.png)

## The red channel property(红色通道属性)

It’s not really nice to hard code the red channel value, so we would like to control the value from the QML side. For this we add a *redChannel* property to our shader effect and also declare a `float redChannel` inside the uniform buffer of the fragment shader. That is all that we need to do to make a value from the QML side available to the shader code.

实际上，硬编码红色通道值并不是很好，所以我们会想从 QML 端控制该值。为此，我们在着色器效果中添加了一个 *redChannel* 属性，并在片段着色器的统一缓冲区中声明了一个 `float redChannel`。这就是我们需要做的，以便将 QML 端的值提供给着色器代码。

::: tip
Notice that the `redChannel` must come after the implicit `qt_Matrix` and `qt_Opacity` in the uniform buffer, `ubuf`. The order of the parameters after the `qt_` parameters is up to you, but `qt_Matrix` and `qt_Opacity` must come first and in that order.

注意，`redChannel` 必须在统一缓冲区 `ubuf` 中的隐式 `qt_Matrix` 和 `qt_Opacity` 之后。`qt_` 参数之后的参数顺序由你决定，但 `qt_Matrix` 和 `qt_Opacity` 必须首先出现，并且顺序是固定的。
:::

<<< @/docs/ch10-effects/src/effects/redlense/2/red3.frag#M1{11}

To make the lens really a lens, we change the *vec4* color to be *vec4(redChannel, 1.0, 1.0, 1.0)* so that the other colors are multiplied by 1.0 and only the red portion is multiplied by our *redChannel* variable.

为了使镜头真正成为镜头，我们将 *vec4* 颜色更改为 *vec4(redChannel, 1.0, 1.0, 1.0)*，以便其他颜色乘以 1.0，而只有红色部分乘以我们的 *redChannel* 变量。


![image](./assets/redlense4.png)

## The red channel animated(红色通道动画)

As the *redChannel* property is just a normal property it can also be animated as all properties in QML. So we can use QML properties to animate values on the GPU to influence our shaders. How cool is that!

正如 *redChannel* 属性只是一个普通的属性一样，它也可以像 QML 中的所有属性一样进行动画处理。因此，我们可以使用 QML 属性来动画化 GPU 上的值，以影响我们的着色器。多么酷啊！

<<< @/docs/ch10-effects/src/effects/redlense/2/redlense2.qml#M1{7-9}

Here the final result.

这是最终结果。

![image](./assets/redlense5.png)

The shader effect on the 2nd row is animated from 0.0 to 1.0 with a duration of 4 seconds. So the image goes from no red information (0.0 red) over to a normal image (1.0 red).

第二行中的着色器效果从 0.0 动画到 1.0，持续时间为 4 秒。因此，图像从没有红色信息（0.0 红色）过渡到正常图像（1.0 红色）。

## Baking（烘焙）

Again, we need to bake the shaders. The following commands from the command line does that:

再次，我们需要烘焙着色器。以下命令行命令可以做到这一点：

```
qsb --glsl 100es,120,150 --hlsl 50 --msl 12 -o red1.frag.qsb red1.frag
qsb --glsl 100es,120,150 --hlsl 50 --msl 12 -o red2.frag.qsb red2.frag
qsb --glsl 100es,120,150 --hlsl 50 --msl 12 -o red3.frag.qsb red3.frag
```
