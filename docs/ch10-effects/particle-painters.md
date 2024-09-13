# Particle Painters（粒子绘制器）

Until now we have only used the image based particle painter to visualize particles. Qt comes also with other particle painters:

到目前为止，我们只使用了基于图像的粒子绘制器来可视化粒子。Qt 还提供了其他粒子绘制器：

* `ItemParticle`: delegate based particle painter（基于委托的粒子绘制器）
* `CustomParticle`: shader based particle painter（基于着色器的粒子绘制器）

The ItemParticle can be used to emit QML items as particles. For this, you need to specify your own delegate to the particle.

ItemParticle 可以用来发射 QML 项目作为粒子。为此，你需要为粒子指定自己的委托。

<<< @/docs/ch10-effects/src/particles/itemparticle.qml#M2

Our delegate, in this case, is a random image (using *Math.random()*), visualized with a white border and a random size.

我们的委托，在这种情况下，是一个随机图像（使用 *Math.random()*），用白色边框和随机大小可视化。

<<< @/docs/ch10-effects/src/particles/itemparticle.qml#M3

We emit 4 images per second with a lifespan of 4 seconds each. The particles fade automatically in and out.

我们每秒发射 4 个图像，每个图像的寿命为 4 秒。粒子会自动淡入淡出。


![image](./assets/itemparticle.png)

For more dynamic cases it is also possible to create an item on your own and let the particle take control of it with `take(item, priority)`. By this, the particle simulation takes control of your particle and handles the item like an ordinary particle. You can get back control of the item by using `give(item)`. You can influence item particles even more by halt their life progression using `freeze(item)` and resume their life using `unfreeze(item)`.

对于更动态的情况，也可以自己创建一个项目，并让粒子使用 `take(item, priority)` 控制它。通过这种方式，粒子模拟控制了你的粒子，并像普通粒子一样处理项目。你可以使用 `give(item)` 恢复对项目的控制。通过使用 `freeze(item)` 暂停粒子的生命进程，并使用 `unfreeze(item)` 恢复粒子的生命，你可以进一步影响项目粒子。
