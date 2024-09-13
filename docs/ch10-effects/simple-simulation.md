# Simple Simulation（简单模拟）

Let us have a look at a very simple simulation to get started. Qt Quick makes it actually very simple to get started with particle rendering. For this we need:

让我们来看一个非常简单的模拟以开始。Qt Quick 实际上让开始粒子渲染变得非常简单。为此我们需要：




* A `ParticleSystem` which binds all elements to a simulation(系统将所有元素绑定到模拟中)
* An `Emitter` which emits particles into the system(发射器将粒子发射到系统中)
* A `ParticlePainter` derived element, which visualizes the particles(派生自 `ParticlePainter` 的元素，用于可视化粒子)

<<< @/docs/ch10-effects/src/particles/simple.qml#M1

The outcome of the example will look like this:

示例的结果如下所示：

![image](./assets/simpleparticles.png)

We start with an 80x80 pixel dark rectangle as our root element and background. Therein we declare a `ParticleSystem`. This is always the first step as the system binds all other elements together. Typically the next element is the `Emitter`, which defines the emitting area based on it’s bounding box and basic parameters for them to be emitted particles. The emitter is bound to the system using the `system` property.

我们从一个 80x80 像素的深色矩形作为根元素和背景开始。在其中我们声明一个 `ParticleSystem`。这是第一步，因为系统将所有其他元素绑定在一起。通常下一个元素是 `Emitter`，它根据它自己的边界框和要发射的粒子基本参数定义发射区域。发射器使用 `system` 属性绑定到系统。

The emitter in this example emits 10 particles per second (`emitRate: 10`) over the area of the emitter with each a lifespan of 1000msec (`lifeSpan : 1000`) and a lifespan variation between emitted particles of 500 msec (`lifeSpanVariation: 500`). A particle shall start with a size of 16px (`size: 16`) and at the end of its life shall be 32px (`endSize: 32`).

示例中的发射器每秒发射 10 个粒子（`emitRate: 10`），在发射区域上每个粒子的寿命为 1000 毫秒（`lifeSpan : 1000`），发射的粒子之间的寿命变化为 500 毫秒（`lifeSpanVariation: 500`）。粒子应以 16px 的尺寸开始（`size: 16`），并在其寿命结束时为 32px（`endSize: 32`）。

The green bordered rectangle is a tracer element to show the geometry of the emitter. This visualizes that also while the particles are emitted inside the emitters bounding box the rendering is not limited to the emitters bounding box. The rendering position depends upon life-span and direction of the particle. This will get more clear when we look into how to change the direction particles.

绿色的边框矩形是一个跟踪元素，用于显示发射器的几何形状。这表明，尽管粒子在发射器的边界框内发射，但渲染并不限于发射器的边界框。渲染位置取决于粒子的寿命和方向。当我们了解如何改变粒子的方向时，这将变得更加清晰。

The emitter emits logical particles. A logical particle is visualized using a `ParticlePainter` in this example we use an `ImageParticle`, which takes an image URL as the source property. The image particle has also several other properties, which control the appearance of the average particle.


发射器发射逻辑粒子。在这个示例中，我们使用 `ParticlePainter` 可视化逻辑粒子。在这个示例中，我们使用 `ImageParticle`，它将图像 URL 作为源属性。图像粒子还有其他一些属性，用于控制平均粒子的外观。


* `emitRate`: particles emitted per second (defaults to 10 per second) (每秒发射的粒子数（默认为每秒 10 个）)
* `lifeSpan`: milliseconds the particle should last for (defaults to 1000 msec) (粒子应持续的时间（默认为 1000 毫秒）
* `size`, `endSize`: size of the particles at the beginning and end of their life  (defaults to 16 px) (粒子寿命开始和结束时的尺寸（默认为 16 像素）

Changing these properties can influence the result in a drastical way

更改这些属性可以极大地影响结果


<<< @/docs/ch10-effects/src/particles/simple2.qml#M1

Besides increasing the emit rate to 40 and the lifespan to 2 seconds the size now starts at 64 pixels and decreases 32 pixels at the end of a particle lifespan.

除了将发射率增加到 40，并将寿命增加到 2 秒钟之外，大小现在从 64 像素开始，并在粒子寿命结束时减少 32 像素。

![image](./assets/simpleparticles2.png)

Increasing the `endSize` even more would lead to a more or less white background. Please note also when the particles are only emitted in the area defined by the emitter the rendering is not constrained to it.

增加 `endSize` 甚至会导致几乎白色的背景。请注意，当粒子仅在发射器定义的区域中发射时，渲染并不受限于它。
