# Vertex Shader（顶点着色器）

The vertex shader can be used to manipulate the vertexes provided by the shader effect. In normal cases, the shader effect has 4 vertexes (top-left, top-right, bottom-left and bottom-right). Each vertex reported is from type `vec4`. To visualize the vertex shader we will program a genie effect. This effect is used to let a rectangular window area vanish into one point, like a genie disappearing into a lamp.

顶点着色器可以用来操控由着色器效果提供的顶点。通常情况下，着色器效果有4个顶点（左上、右上、左下和右下）。报告的每个顶点都是 `vec4` 类型。为了演示顶点着色器，我们将编程一个精灵效果。这种效果用于让一个矩形窗口区域消失在一个点中，就像一个精灵消失在灯里一样。



![image](./assets/genieeffect.png)

## Setting up the scene（设置场景）

First, we will set up our scene with an image and a shader effect.

首先，我们将使用图像和着色器效果来设置场景。

<<< @/docs/ch10-effects/src/effects/genie/0/genie0.qml#M1

This provides a scene with a dark background and a shader effect using an image as the source texture. The original image is not visible on the image produced by our genie effect. Additional we added a dark rectangle on the same geometry as the shader effect so we can better detect where we need to click to revert the effect.

这提供了一个带有深色背景的图像和着色器效果，使用图像作为源纹理。原始图像在我们的精灵效果生成的图像中是不可见的。另外，我们在与着色器效果相同的几何体上添加了一个深色矩形，以便我们可以更好地检测需要点击以反转效果的地方。

![image](./assets/geniescene.png)

The effect is triggered by clicking on the image, this is defined by the mouse area covering the effect. In the *onClicked* handler we toggle the custom boolean property *minimized*. We will use this property later to toggle the effect.

效果是通过点击图像触发的，这是由覆盖效果的鼠标区域定义的。在 *onClicked* 处理程序中，我们切换自定义布尔属性 *minimized*。稍后我们将使用此属性切换效果。

## Minimize and normalize

After we have set up the scene, we define a property of type real called *minimize*, the property will contain the current value of our minimization. The value will vary from 0.0 to 1.0 and is controlled by a sequential animation.

在设置好场景后，我们定义了一个名为 *minimize* 的实数类型属性，该属性将包含我们最小化的当前值。该值将在 0.0 到 1.0 之间变化，并由顺序动画控制。


<<< @/docs/ch10-effects/src/effects/genie/1/genie1.qml#M1

The animation is triggered by the toggling of the *minimized* property. Now that we have set up all our surroundings we finally can look at our vertex shader.

通过切换 *minimized* 属性来触发动画。现在我们已经设置好了所有环境，我们终于可以看看我们的顶点着色器了。


<<< @/docs/ch10-effects/src/effects/genie/1/genie1.vert#M1

The vertex shader is called for each vertex so four times, in our case. The default qt defined parameters are provided, like *qt_Matrix*, *qt_Vertex*, *qt_MultiTexCoord0*, *qt_TexCoord0*. We have discussed the variable already earlier. Additional we link the *minimize*, *width* and *height* variables from our shader effect into our vertex shader code. In the main function, we store the current texture coordinate in our *qt_TexCoord0* to make it available to the fragment shader. Now we copy the current position and modify the x and y position of the vertex:

顶点着色器在每个顶点上都会被调用，在我们的情况下就是四次。默认的 Qt 定义的参数会被提供，比如 *qt_Matrix*、*qt_Vertex*、*qt_MultiTexCoord0*、*qt_TexCoord0*。我们之前已经讨论过这些变量。另外，我们将着色器效果中的 *minimize*、*width* 和 *height* 变量链接到我们的顶点着色器代码中。在主函数中，我们将当前的纹理坐标存储在 *qt_TexCoord0* 中，以便在片段着色器中可用。现在我们复制当前位置并修改顶点的 x 和 y 坐标：



```glsl
vec4 pos = qt_Vertex;
pos.y = mix(qt_Vertex.y, ubuf.height, ubuf.minimize);
pos.x = mix(qt_Vertex.x, ubuf.width, ubuf.minimize);
```

The `mix(…)` function provides a linear interpolation between the first 2 parameters on the point (0.0-1.0) provided by the 3rd parameter. So in our case, we interpolate for y between the current y position and the height based on the current minimized value, similar for x. Bear in mind the minimized value is animated by our sequential animation and travels from 0.0 to 1.0 (or vice versa).

`mix(…)` 函数提供了第一个和第二个参数之间的线性插值，基于第三个参数提供的 (0.0-1.0) 点。在我们的情况下，我们根据当前的最小化值在当前 y 位置和高度之间进行插值，x 也类似。请记住，最小化值是由我们的顺序动画动画化的，从 0.0 到 1.0（或反之）。


![image](./assets/genieminimize.png)

The resulting effect is not really the genie effect but is already a great step towards it.

最终效果并不是真正的精灵效果，但已经是一个朝着它的巨大进步了。

## Primitive Bending（基本弯曲）

So minimized the x and y components of our vertexes. Now we would like to slightly modify the x manipulation and make it depending on the current y value. The needed changes are pretty small. The y-position is calculated as before. The interpolation of the x-position depends now on the vertexes y-position:

我们已经最小化了顶点的 x 和 y 组件。现在我们希望稍微修改 x 的操作，并使其取决于当前的 y 值。需要的更改非常小。y 位置的计算与之前相同。x 位置的插值现在取决于顶点的 y 位置：

```glsl
float t = pos.y / ubuf.height;
pos.x = mix(qt_Vertex.x, ubuf.width, t * minimize);
```

This results in an x-position tending towards the width when the y-position is larger. In other words, the upper 2 vertexes are not affected at all as they have a y-position of 0 and the lower two vertexes x-positions both bend towards the width, so they bend towards the same x-position.

当 y 位置较大时，x 位置会趋向于宽度。换句话说，上方的两个顶点根本不受影响，因为它们的 y 位置为 0，而下面的两个顶点的 x 位置都弯曲向宽度，所以它们弯曲向相同的 x 位置。

![image](./assets/geniebending.png)

## Better Bending（更好的弯曲）

As the bending is not really satisfying currently we will add several parts to improve the situation.
First, we enhance our animation to support an own bending property. This is necessary as the bending should happen immediately and the y-minimization should be delayed shortly. Both animations have in the sum the same duration (300+700+1000 and 700+1300).

我们首先增强我们的动画，以支持自己的弯曲属性。这是必要的，因为弯曲应该立即发生，而 y 最小化应该稍后延迟。两个动画的总持续时间相同（300+700+1000 和 700+1300）。


We first add and animate `bend` from QML.

我们首先在 QML 中添加并动画化 `bend`。


<<< @/docs/ch10-effects/src/effects/genie/3/genie3.qml#M1

We then add `bend` to the uniform buffer, `ubuf` and use it in the shader to achieve a smoother bending.

然后我们将 `bend` 添加到统一缓冲区 `ubuf` 中，并在着色器中使用它以实现更平滑的弯曲。


<<< @/docs/ch10-effects/src/effects/genie/3/genie3.vert#M1

The curve starts smooth at the 0.0 value, grows then and stops smoothly towards the 1.0 value. Here is a plot of the function in the specified range. For us, only the range from 0..1 is from interest.

曲线在 0.0 值时开始平滑，然后增长，并在 1.0 值时平滑停止。以下是该函数在指定范围内的图表。对我们来说，只有 0..1 的范围是感兴趣的。

![image](./assets/curve.png)

We also need to increase the number of vertex points. The vertex points used can be increased by using a mesh.

我们还需要增加顶点的数量。顶点点数可以通过使用网格来增加。


```qml
mesh: GridMesh { resolution: Qt.size(16, 16) }
```

The shader effect now has an equality distributed grid of 16x16 vertexes instead of the 2x2 vertexes used before. This makes the interpolation between the vertexes look much smoother.

着色器效果现在有 16x16 个顶点的均匀分布网格，而不是之前使用的 2x2 个顶点。这使得顶点之间的插值看起来要平滑得多。

![image](./assets/geniesmoothbending.png)

You can see also the influence of the curve being used, as the bending smoothes at the end nicely. This is where the bending has the strongest effect.

你也可以看到所使用的曲线的影响，因为弯曲在最后非常平滑。这是弯曲最强的地方。

## Choosing Sides（选择侧面）

As a final enhancement, we want to be able to switch sides. The side is towards which point the genie effect vanishes. Until now it vanishes always towards the width. By adding a `side` property we are able to modify the point between 0 and width.

作为最后的增强，我们希望能够切换侧面。侧面是指 genie 效果消失的方向。到目前为止，它总是向宽度消失。通过添加一个 `side` 属性，我们可以修改 0 和宽度之间的点。


```qml
ShaderEffect {
    ...
    property real side: 0.5
    ...
}
```

<<< @/docs/ch10-effects/src/effects/genie/4/genie4.vert#M1

![image](./assets/geniehalfside.png)

## Packaging（打包）

The last thing to-do is packaging our effect nicely. For this, we extract our genie effect code into an own component called `GenieEffect`. It has the shader effect as the root element. We removed the mouse area as this should not be inside the component as the triggering of the effect can be toggled by the `minimized` property.

最后要做的事情是将我们的效果打包得很好。为此，我们将 genie 效果代码提取到一个名为 `GenieEffect` 的独立组件中。它将着色器效果作为根元素。我们删除了鼠标区域，因为触发效果应该不在组件内部，因为可以通过 `minimized` 属性切换效果。

<<< @/docs/ch10-effects/src/effects/genie/demo/GenieEffect.qml#M1
<<< @/docs/ch10-effects/src/effects/genie/demo/genieeffect.vert#M1

You can use now the effect simply like this:

<<< @/docs/ch10-effects/src/effects/genie/demo/geniedemo.qml#M1

We have simplified the code by removing our background rectangle and we assigned the image directly to the effect, instead of loading it inside a standalone image element.

我们通过删除我们的背景矩形来简化了代码，并将图像直接分配给效果，而不是在独立的图像元素中加载它。