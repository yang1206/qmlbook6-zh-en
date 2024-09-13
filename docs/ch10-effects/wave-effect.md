# Wave Effect（波浪效果）

In this more complex example, we will create a wave effect with the fragment shader. The waveform is based on the sinus curve and it influences the texture coordinates used for the color.

在这个更复杂的例子中，我们将使用片段着色器创建一个波浪效果。波形基于正弦曲线，并影响用于颜色的纹理坐标。

The qml file defines the properties and animation.

这个qml文件定义了属性和动画。

<<< @/docs/ch10-effects/src/effects/wave/wave.qml#M1

The fragment shader takes the properties and calculates the color of each pixel based on the properties.

片段着色器接受属性并根据属性计算每个像素的颜色。

<<< @/docs/ch10-effects/src/effects/wave/wave.frag#M1

The wave calculation is based on a pulse and the texture coordinate manipulation. The pulse equation gives us a sine wave depending on the current time and the used texture coordinate:

波形计算基于脉冲和纹理坐标操作。脉冲方程根据当前时间和使用的纹理坐标给出正弦波：

```glsl
vec2 pulse = sin(ubuf.time - ubuf.frequency * qt_TexCoord0);
```

Without the time factor, we would just have a distortion but not a traveling distortion like waves are.

没有时间因素，我们只会扭曲，而不是像波浪那样移动的扭曲。


For the color we use the color at a different texture coordinate:

对于颜色，我们使用不同纹理坐标的颜色：


```glsl
vec2 coord = qt_TexCoord0 + ubuf.amplitude * vec2(pulse.x, -pulse.x);
```

The texture coordinate is influenced by our pulse x-value. The result of this is a moving wave.

纹理坐标受到脉冲x值的影响。结果是一个移动的波浪。


![image](./assets/wave.png)

In this example we use a fragment shader, meaning that we move the pixels inside the texture of the rectangular item. If we wanted the entire item to move as a wave we would have to use a vertex shader.

在这个例子中，我们使用了一个片段着色器，这意味着我们在矩形的纹理中移动像素。如果我们想让整个项目作为一个波浪移动，我们就必须使用一个顶点着色器。
