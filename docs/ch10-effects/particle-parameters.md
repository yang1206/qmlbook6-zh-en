# Particle Parameters(粒子参数)

We saw already how to change the behavior of the emitter to change our simulation. The particle painter used allows us how the particle image is visualized for each particle.

我们已经看到了如何改变发射器的行为来改变我们的模拟。粒子绘制器允许我们如何为每个粒子可视化粒子图像。

Coming back to our example we update our `ImageParticle`. First, we change our particle image to a small sparking star image:

回到我们的例子中，我们更新我们的`ImageParticle`。首先，我们将粒子图像更改为一个小火花星图像：

```qml
ImageParticle {
    ...
    source: 'assets/star.png'
}
```

The particle shall be colorized in an gold color which varies from particle to particle by +/- 20%:

粒子将被着色为金色，每个粒子的变化范围为 +/- 20%：

```qml
color: '#FFD700'
colorVariation: 0.2
```

To make the scene more alive we would like to rotate the particles. Each particle should start by 15 degrees clockwise and varies between particles by +/-5 degrees. Additional the particle should continuously rotate with the velocity of 45 degrees per second. The velocity shall also vary from particle to particle by +/- 15 degrees per second:

为了使场景更加生动，我们希望旋转粒子。每个粒子应该从顺时针15度开始，每个粒子的变化范围为 +/-5度。另外，粒子应该以每秒45度的速度连续旋转。速度也应该从粒子到粒子变化 +/- 15度/秒：

```qml
rotation: 15
rotationVariation: 5
rotationVelocity: 45
rotationVelocityVariation: 15
```

Last but not least, we change the entry effect for the particle. This is the effect used when a particle comes to life. In this case, we want to use the scale effect:

最后但并非最不重要的是，我们改变了粒子的入口效果。这是粒子开始生命时使用的效果。在这种情况下，我们想使用缩放效果：

```qml
entryEffect: ImageParticle.Scale
```

So now we have rotating golden stars appearing all over the place.

所以现在我们有了旋转的金色星星出现在各个地方。

![image](./assets/particleparameters.png)

Here is the code we changed for the image-particle in one block.

这是我们为图像粒子更改的代码块。

<<< @/docs/ch10-effects/src/particles/particlevariation.qml#M1
