# Particle Groups（粒子组）

At the beginning of this chapter, we stated particles are in groups, which is by default the empty group (‘’). Using the `GroupGoal` affector is it possible to let the particle change groups. To visualize this we would like to create a small firework, where rockets start into space and explode in the air into a spectacular firework.

 在本章的开头，我们声明粒子在组中，默认情况下是空组（‘’）。使用 `GroupGoal` 作用器，可以让粒子改变组。为了可视化这一点，我们想创建一个小烟花，其中火箭开始进入太空，并在空中爆炸成壮观的花火。

![image](./assets/firework_teaser.png)

The example is divided into 2 parts. The 1st part called “Launch Time” is concerned to set up the scene and introduce particle groups and the 2nd part called “Let there be fireworks” focuses on the group changes.

该示例分为两部分。第一部分称为“发射时间”，负责设置场景并介绍粒子组；第二部分称为“让烟火绽放吧”，专注于组的变化。

Let’s get started!

让我们开始吧！

## Launch Time

To get it going we create a typical dark scene:

要开始，我们创建一个典型的黑暗场景：

```qml
import QtQuick 2.5
import QtQuick.Particles 2.0

Rectangle {
    id: root
    width: 480; height: 240
    color: "#1F1F1F"
    property bool tracer: false
}
```

The tracer property will be used to switch the tracer scene wide on and off. The next thing is to declare our particle system:

`tracer` 属性将被用来在整个场景中打开和关闭追踪器。接下来要做的是声明我们的粒子系统：

```qml
ParticleSystem {
    id: particleSystem
}
```

And our two image particles (one for the rocket and one for the exhaust smoke):

我们的两个图像粒子（一个用于火箭，一个用于排烟）：


```qml
ImageParticle {
    id: smokePainter
    system: particleSystem
    groups: ['smoke']
    source: "assets/particle.png"
    alpha: 0.3
    entryEffect: ImageParticle.None
}

ImageParticle {
    id: rocketPainter
    system: particleSystem
    groups: ['rocket']
    source: "assets/rocket.png"
    entryEffect: ImageParticle.None
}
```

You can see in on the images, they use the *groups* property to declare to which group the particle belongs. It is enough to just declare a name and an implicit group will be created by Qt Quick.

您可以在图像中看到，它们使用 *groups* 属性来声明粒子属于哪个组。只需声明一个名称，Qt Quick 就会创建一个隐式组。


Now it’s time to emit some rockets into the air. For this, we create an emitter on the bottom of our scene and set the velocity in an upward direction. To simulate some gravity we set an acceleration downwards:

现在，是时候发射一些火箭进入空中了。为此，我们在场景底部创建一个发射器，并将速度设置为向上方向。为了模拟一些重力，我们设置一个向下的加速度：

```qml
Emitter {
    id: rocketEmitter
    anchors.bottom: parent.bottom
    width: parent.width; height: 40
    system: particleSystem
    group: 'rocket'
    emitRate: 2
    maximumEmitted: 4
    lifeSpan: 4800
    lifeSpanVariation: 400
    size: 32
    velocity: AngleDirection { angle: 270; magnitude: 150; magnitudeVariation: 10 }
    acceleration: AngleDirection { angle: 90; magnitude: 50 }
    Tracer { color: 'red'; visible: root.tracer }
}
```

The emitter is in the group *‘rocket’*, the same as our rocket particle painter. Through the group name, they are bound together. The emitter emits particles into the group ‘rocket’ and the rocket particle painter will pain them.

发射器在 *‘rocket’* 组中，与我们的火箭粒子绘制器相同。通过组名，它们是绑在一起的。发射器将粒子发射到‘rocket’组中，火箭粒子绘制器将绘制它们。

For the exhaust, we use a trail emitter, which follows our rocket. It declares an own group called ‘smoke’ and follows the particles from the ‘rocket’ group:

为了排烟，我们使用尾迹发射器，它跟随我们的火箭。它声明了一个名为‘smoke’的自己的组，并跟随来自‘rocket’组的粒子：



```qml
TrailEmitter {
    id: smokeEmitter
    system: particleSystem
    emitHeight: 1
    emitWidth: 4
    group: 'smoke'
    follow: 'rocket'
    emitRatePerParticle: 96
    velocity: AngleDirection { angle: 90; magnitude: 100; angleVariation: 5 }
    lifeSpan: 200
    size: 16
    sizeVariation: 4
    endSize: 0
}
```

The smoke is directed downwards to simulate the force the smoke comes out of the rocket. The *emitHeight* and *emitWidth* specify the are around the particle followed from where the smoke particles shall be emitted. If this is not specified then they are of the particle followed is taken but for this example, we want to increase the effect that the particles stem from a central point near the end of the rocket.

排烟是向下方向的，以模拟烟雾从火箭中出来的力。*emitHeight* 和 *emitWidth* 指定从跟随粒子的周围区域发射烟雾粒子的区域。如果未指定，则采用跟随粒子的区域，但对于这个例子，我们希望增加粒子从火箭末端附近的一个中心点发出的效果。

If you start the example now you will see the rockets fly up and some are even flying out of the scene. As this is not really wanted we need to slow them down before they leave the screen. A friction affector can be used here to slow the particles down to a minimum threshold:

如果您现在启动示例，您将看到火箭飞向空中，甚至有些火箭飞出了场景。这不是我们想要的，所以我们需要在它们离开屏幕之前减慢它们的速度。这里可以使用摩擦力作用器来减慢粒子的速度到最小阈值：

```qml
Friction {
    groups: ['rocket']
    anchors.top: parent.top
    width: parent.width; height: 80
    system: particleSystem
    threshold: 5
    factor: 0.9
}
```

In the friction affector, you also need to declare which groups of particles it shall affect. The friction will slow all rockets, which are 80 pixels downwards from the top of the screen down by a factor of 0.9 (try 100 and you will see they almost stop immediately) until they reach a velocity of 5 pixels per second. As the particles have still an acceleration downwards applied the rockets will start sinking toward the ground after they reach the end of their life-span.

在摩擦力作用器中，您还需要声明它将影响哪些粒子组。摩擦力将减慢所有从屏幕顶部向下80像素的火箭，以0.9的系数减慢速度（尝试100，您将看到它们几乎立即停止），直到它们达到每秒5像素的速度。由于粒子仍然有一个向下的加速度，火箭将在达到寿命结束时开始向地面下沉。

As climbing up in the air is hard work and a very unstable situation we want to simulate some turbulences while the ship is climbing:

由于在空中爬升是一项非常艰巨的工作，而且是一种非常不稳定的情况，我们想在飞船爬升时模拟一些 turbulence：


```qml
Turbulence {
    groups: ['rocket']
    anchors.bottom: parent.bottom
    width: parent.width; height: 160
    system: particleSystem
    strength: 25
    Tracer { color: 'green'; visible: root.tracer }
}
```

Also, the turbulence needs to declare which groups it shall affect. The turbulence itself reaches from the bottom 160 pixels upwards (until it reaches the border of the friction). They also could overlap.

此外，需要声明湍流将影响哪些组。湍流本身从底部的160像素向上延伸（直到达到摩擦力的边界）。它们也可能会重叠。



When you start the example now you will see the rockets are climbing up and then will be slowed down by the friction and fall back to the ground by the still applied downwards acceleration. The next thing would be to start the firework.

现在启动示例，您将看到火箭正在上升，然后由于仍然应用的向下加速度而开始下沉。接下来，我们可以开始放烟花。


![image](./assets/firework_rockets.png)

::: tip
The image shows the scene with the tracers enabled to show the different areas. Rocket particles are emitted in the red area and then affected by the turbulence in the blue area. Finally, they are slowed down by the friction affector in the green area and start falling again, because of the steady applied downwards acceleration.


图像显示了启用追踪器的场景，以显示不同的区域。火箭粒子在红色区域中发射，然后受到蓝色区域中的湍流的影响。最后，它们受到绿色区域中摩擦力作用器的减慢，并开始再次下落，因为持续应用的向下加速度。
:::

## Let there be fireworks（放烟花）

To be able to change the rocket into a beautiful firework we need add a `ParticleGroup` to encapsulate the changes:

为了能够将火箭变成美丽的烟花，我们需要添加一个 `ParticleGroup` 来封装这些变化：


```qml
ParticleGroup {
    name: 'explosion'
    system: particleSystem
}
```

We change to the particle group using a `GroupGoal` affector. The group goal affector is placed near the vertical center of the screen and it will affect the group ‘rocket’. With the *groupGoal* property we set the target group for the change to ‘explosion’, our earlier defined particle group:

我们使用 `GroupGoal` 作用器来改变粒子组。组目标作用器放置在屏幕的垂直中心附近，它将影响组 'rocket'。通过 `groupGoal` 属性，我们将目标组更改为 'explosion'，我们之前定义的粒子组：

```qml
GroupGoal {
    id: rocketChanger
    anchors.top: parent.top
    width: parent.width; height: 80
    system: particleSystem
    groups: ['rocket']
    goalState: 'explosion'
    jump: true
    Tracer { color: 'blue'; visible: root.tracer }
}
```

The *jump* property states the change in groups shall be immediately and not after a certain duration.

*jump* 属性表示组的变化应立即进行，而不是在一段时间后进行。

::: tip
In the Qt 5 alpha release we could the *duration* for the group change not get working. Any ideas?

在 Qt 5 alpha 版本中，我们无法使 *duration* 组变化正常工作。有什么想法吗？
:::

As the group of the rocket now changes to our ‘explosion’ particle group when the rocket particle enters the group goal area we need to add the firework inside the particle group:

当火箭粒子进入组目标区域时，火箭粒子的组将更改为我们的 'explosion' 粒子组，我们需要在粒子组中添加烟花：


```qml
// inside particle group
TrailEmitter {
    id: explosionEmitter
    anchors.fill: parent
    group: 'sparkle'
    follow: 'rocket'
    lifeSpan: 750
    emitRatePerParticle: 200
    size: 32
    velocity: AngleDirection { angle: -90; angleVariation: 180; magnitude: 50 }
}
```

The explosion emits particles into the ‘sparkle’ group. We will define soon a particle painter for this group. The trail emitter used follows the rocket particle and emits per rocket 200 particles. The particles are directed upwards and vary by 180 degrees.

爆炸将粒子发射到 'sparkle' 组中。我们很快将为这个组定义一个粒子绘制器。轨迹发射器跟随火箭粒子，每个火箭发射200个粒子。粒子向上发射，并变化180度。


As the particles are emitted into the ‘sparkle’ group, we also need to define a particle painter for the particles:

由于粒子被发射到 'sparkle' 组中，我们还需要为粒子定义一个粒子绘制器：

```qml
ImageParticle {
    id: sparklePainter
    system: particleSystem
    groups: ['sparkle']
    color: 'red'
    colorVariation: 0.6
    source: "assets/star.png"
    alpha: 0.3
}
```

The sparkles of our firework shall be little red stars with an almost transparent color to allow some shine effects.

我们的烟花火花应该是小红色的星星，几乎透明的颜色以允许一些闪光效果。


To make the firework more spectacular we also add a second trail emitter to our particle group, which will emit particles in a narrow cone downwards:

为了使烟花更加壮观，我们还在粒子组中添加了第二个轨迹发射器，它将向下发射粒子锥形：


```qml
// inside particle group
TrailEmitter {
    id: explosion2Emitter
    anchors.fill: parent
    group: 'sparkle'
    follow: 'rocket'
    lifeSpan: 250
    emitRatePerParticle: 100
    size: 32
    velocity: AngleDirection { angle: 90; angleVariation: 15; magnitude: 400 }
}
```

Otherwise, the setup is similar to the other explosion trail emitter. That’s it.

否则，设置与另一个爆炸轨迹发射器类似。就是这样。


Here is the final result.

这是最终结果。



![image](./assets/firework_final.png)

Here is the full source code of the rocket firework.

这是火箭烟花完整的源代码。

<<< @/docs/ch10-effects/src/particles/firework.qml#M1
