# Directed Particles（有向粒子）

We have seen particles can rotate. But particles can also have a trajectory. The trajectory is specified as the velocity or acceleration of particles defined by a stochastic direction also named a vector space.

我们已经看到粒子可以旋转。但是粒子也可以有轨迹。轨迹由粒子速度或加速度定义，速度或加速度由随机方向定义，也称为向量空间。

There are different vector spaces available to define the velocity or acceleration of a particle:

可以使用不同的向量空间来定义粒子的速度或加速度：


* `AngleDirection` - a direction that varies in angle（角度变化的方向）
* `PointDirection` - a direction that varies in x and y components（x和y分量变化的方向）
* `TargetDirection` - a direction towards the target point（朝向目标点的方向）

![image](./assets/particle_directions.png)

Let’s try to move the particles over from the left to the right side of our scene by using the velocity directions.

让我们尝试使用速度方向将粒子从场景的左侧移动到右侧。


We first try the `AngleDirection`. For this we need to specify the `AngleDirection` as an element of the velocity property of our emitter:

首先，我们尝试使用`AngleDirection`。为此，我们需要将`AngleDirection`指定为发射器速度属性的一个元素：


```qml
velocity: AngleDirection { }
```

The angle where the particles are emitted is specified using the angle property. The angle is provided as a value between 0..360 degree and 0 points to the right. For our example, we would like the particles to move to the right so 0 is already the right direction. The particles shall spread by +/- 5 degrees:

使用角度属性指定发射粒子的角度。角度以0..360度提供，0指向右侧。对于我们的示例，我们希望粒子向右移动，所以0已经是正确的方向。粒子将根据+/- 5度扩散：

```qml
velocity: AngleDirection {
    angle: 0
    angleVariation: 15
}
```

Now we have set our direction, the next thing is to specify the velocity of the particle. This is defined by a magnitude. The magnitude is defined in pixels per seconds. As we have ca. 640px to travel 100 seems to be a good number. This would mean by an average lifetime of 6.4 secs a particle would cross the open space. To make the traveling of the particles more interesting we vary the magnitude using the `magnitudeVariation` and set this to the half of the magnitude:

现在我们已经设置了方向，接下来要指定粒子的速度。这由速度大小定义。速度大小以每秒像素为单位定义。由于大约有640px需要穿越，所以100似乎是一个很好的数字。这意味着平均寿命为6.4秒的粒子将穿越开放空间。为了使粒子的旅行更有趣，我们使用`magnitudeVariation`来变化速度大小，并将其设置为速度大小的一半：


```qml
velocity: AngleDirection {
    ...
    magnitude: 100
    magnitudeVariation: 50
}
```

![image](./assets/angledirection.png)

Here is the full source code, with an average lifetime set to 6.4 seconds. We set the emitter width and height to 1px. This means all particles are emitted at the same location and from thereon travel based on our given trajectory.

这是完整的源代码，平均寿命设置为6.4秒。我们将发射器宽度和高度设置为1px。这意味着所有粒子都从同一位置发射，并据此进行旅行。

<<< @/docs/ch10-effects/src/particles/angledirection.qml#M1

So what is then the acceleration doing? The acceleration adds an acceleration vector to each particle, which changes the velocity vector over time. For example, let’s make a trajectory like an arc of stars. For this we change our velocity direction to -45 degree and remove the variations, to better visualize a coherent arc:

那么加速度在做什么呢？加速度为每个粒子添加一个加速度向量，该向量随时间改变速度向量。例如，让我们制作一个像星星的弧形轨迹。为此，我们将速度方向更改为-45度，并删除变化，以便更好地可视化连贯的弧形：

```qml
velocity: AngleDirection {
    angle: -45
    magnitude: 100
}
```

The acceleration direction shall be 90 degrees (down direction) and we choose one-fourth of the velocity magnitude for this:


加速度方向应为90度（向下方向），我们选择速度大小的一四分之一：


```qml
acceleration: AngleDirection {
    angle: 90
    magnitude: 25
}
```

The result is an arc going from the center-left to the bottom right.

结果是一个从中心左侧到底部右侧的弧形。

![image](./assets/angledirection2.png)

The values are discovered by trial-and-error.

这些值是通过试错发现的。


Here is the full code of our emitter.

这是我们的发射器的完整代码。


<<< @/docs/ch10-effects/src/particles/angledirection2.qml#M1

In the next example we would like that the particles again travel from left to right but this time we use the `PointDirection` vector space.

在下一个示例中，我们希望粒子再次从左向右移动，但这次我们使用`PointDirection`向量空间。


A `PointDirection` derived its vector space from an x and y component. For example, if you want the particles to travel in a 45-degree vector, you need to specify the same value for x and y.

`PointDirection`从x和y分量派生出其向量空间。例如，如果你想使粒子以45度向量移动，你需要为x和y指定相同的值。

In our case we want the particles to travel from left-to-right building a 15-degree cone. For this we specify a `PointDirection` as our velocity vector space:

在我们的例子中，我们希望粒子从左向右移动，形成一个15度的锥形。为此，我们将`PointDirection`指定为我们的速度向量空间：


```qml
velocity: PointDirection { }
```

To achieve a traveling velocity of 100 px per seconds we set our x component to 100. For the 15 degrees (which is 1/6th of 90 degrees) we specify any variation of 100/6:

为了实现每秒100px的旅行速度，我们将x分量设置为100。对于15度（即90度的一六分之一），我们指定任何100/6的变化：


```qml
velocity: PointDirection {
    x: 100
    y: 0
    xVariation: 0
    yVariation: 100/6
}
```

The result should be particles traveling in a 15-degree cone from right to left.

结果应该是粒子从右向左以15度的锥形旅行。


![image](./assets/pointdirection.png)

Now coming to our last contender, the `TargetDirection`. The target direction allows us to specify a target point as an x and y coordinate relative to the emitter or an item. When an item has specified the center of the item will become the target point. You can achieve the 15-degree cone by specifying a target variation of 1/6 th of the x target:

现在我们来看最后一个竞争者，`TargetDirection`。目标方向允许我们指定一个相对于发射器或项目的x和y坐标的目标点。当一个项目指定了项目的中心时，该中心将成为目标点。你可以通过指定目标变化为x目标的1/6来获得15度的锥形：

```qml
velocity: TargetDirection {
    targetX: 100
    targetY: 0
    targetVariation: 100/6
    magnitude: 100
}
```

::: tip
Target direction are great to use when you have a specific x/y coordinate you want the stream of particles emitted towards.

当您有特定的x/y坐标，您希望粒子流朝该坐标发射时，目标方向非常有用。
:::

I spare you the image as it looks the same as the previous one, instead, I have a quest for you.

我节省了图像，因为它看起来与上一个图像相同，相反，我有一个问题要问你。

In the following image, the red and the green circle specify each a target item for the target direction of the velocity respective the acceleration property. Each target direction has the same parameters. Here the question: Who is responsible for velocity and who is for acceleration?

在下面的图像中，红色和绿色圆圈分别指定了速度和加速度属性的目标方向的目标项目。每个目标方向都有相同的参数。这里的问题：谁负责速度，谁负责加速度？


![image](./assets/directionquest.png)
