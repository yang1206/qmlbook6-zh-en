# Curtain Effect（窗帘效果）

In the last example for custom shader effects, I would like to bring you the curtain effect. This effect was published first in May 2011 as part of [Qt labs for shader effects](http://labs.qt.nokia.com/2011/05/03/qml-shadereffectitem-on-qgraphicsview/).

在最后一个自定义着色器效果示例中，我想带您了解窗帘效果。这个效果最早在2011年5月作为[着色器效果实验室](http://labs.qt.nokia.com/2011/05/03/qml-shadereffectitem-on-qgraphicsview/)的一部分发布。

![image](./assets/curtain.png)

At that time I really loved these effects and the curtain effect was my favorite out of them. I just love how the curtain opens and hide the background object.

那时我真的喜欢这些效果，窗帘效果是我最喜欢的。我真的很喜欢窗帘打开时隐藏背景对象。

For this chapter, the effect has been adapted for Qt 6. It has also been slightly simplified to make it a better showcase.

对于本章，效果已经适应了Qt 6。它也被稍微简化了一些，以便更好地展示。

The curtain image is called `fabric.png`. The effect then uses a vertex shader to swing the curtain forth and back and a fragment shader to apply shadows to show how the fabric folds.

窗帘图像被称为`fabric.png`。效果然后使用顶点着色器来回摆动窗帘，并使用片段着色器应用阴影，以显示布料如何折叠。

The diagram below shows how the shader works. The waves are computed through a sin curve with 7 periods (7\*PI=21.99…). The other part is the swinging. The `topWidht` of the curtain is animated when the curtain is opened or closed. The `bottomWidth` follows the `topWidth` using a `SpringAnimation`. This creates the effect of the bottom part of the curtain swinging freely. The calculated `swing` component is the strength of the swing based on the y-component of the vertexes.


下图显示了着色器的工作原理。波浪是通过7个周期（7\*PI=21.99…）的正弦曲线计算的。另一部分是摆动。当窗帘打开或关闭时，窗帘的`topWidht`被动画化。`bottomWidth`使用`SpringAnimation`跟随`topWidth`。这创建了窗帘底部自由摆动的效果。计算的`swing`组件是基于顶点的y分量计算的摆动强度。

![image](./assets/curtain_diagram.png)

The curtain effect is implemented in the `CurtainEffect.qml` file where the fabric image act as the texture source. In the QML code, the `mesh` property is adjusted to make sure that the number of vertices is increased to give a smoother result.


窗帘效果在`CurtainEffect.qml`文件中实现，其中布料图像作为纹理源。在QML代码中，`mesh`属性被调整，以确保顶点数量增加，以获得更平滑的结果。


<<< @/docs/ch10-effects/src/effects/curtain/CurtainEffect.qml#M1

The vertex shader, shown below, reshapes the curtain based on the `topWidth` and `bottomWidth` properties, extrapolating the shift based on the y-coordinate. It also calculates the `shade` value, which is used in the fragment shader. The `shade` property is passed through an additional output in `location` 1.

下面的顶点着色器根据`topWidth`和`bottomWidth`属性重新塑造窗帘，根据y坐标外推位移。它还计算`shade`值，该值在片段着色器中使用。`shade`属性通过`location` 1中的额外输出来传递。

<<< @/docs/ch10-effects/src/effects/curtain/curtain.vert#M1

In the fragment shader below, the `shade` is picked up as an input in `location` 1 and is then used to calculate the `fragColor`, which is used to draw the pixel in question.

下面的片段着色器中，`shade`作为`location` 1中的输入被拾取，然后用于计算`fragColor`，该值用于绘制所讨论的像素。

<<< @/docs/ch10-effects/src/effects/curtain/curtain.frag#M1

The combination of QML animations and passing variables from the vertex shader to the fragment shader demonstrates how QML and shaders can be used together to build complex, animated, effects.

QML动画和将变量从顶点着色器传递到片段着色器的组合展示了QML和着色器如何一起使用来构建复杂、动画的效果。

The effect itself is used from the `curtaindemo.qml` file shown below.

效果本身是从下面的`curtaindemo.qml`文件中使用的。

<<< @/docs/ch10-effects/src/effects/curtain/curtaindemo.qml#M1

The curtain is opened through a custom `open` property on the curtain effect. We use a `MouseArea` to trigger the opening and closing of the curtain when the user clicks or taps the area.

窗帘通过窗帘效果上的自定义`open`属性打开。我们使用`MouseArea`在用户点击或轻触区域时触发窗帘的打开和关闭。
