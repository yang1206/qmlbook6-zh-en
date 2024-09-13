# Particle Concept（粒子概念）

In the heart of the particle simulation is the `ParticleSystem` which controls the shared timeline. A scene can have several particles systems, each of them with an independent time-line. A particle is emitted using an `Emitter` element and visualized with a `ParticlePainter`, which can be an image, QML item or a shader item.
An emitter provides also the direction for particle using a vector space. Particle ones emitted can’t be manipulated by the emitter anymore. The particle module provides the `Affector`, which allows manipulating parameters of the particle after it has been emitted.

粒子模拟的核心是 `ParticleSystem`，它控制共享的时间线。一个场景可以有多个粒子系统，每个系统都有一个独立的时间线。粒子通过 `Emitter` 元素发射，并通过 `ParticlePainter` 可视化，它可以是一张图片、一个QML项或一个着色器项。发射器还提供了使用向量空间来为粒子提供方向。一旦粒子被发射，就无法再由发射器操控。粒子模块提供了 `Affector`，它允许在粒子发射后操控粒子的参数。



Particles in a system can share timed transitions using the `ParticleGroup` element. By default, every particle is on the empty (‘’) group.

系统中粒子的参数可以通过 `ParticleGroup` 元素共享定时过渡。默认情况下，每个粒子都属于空（‘’）组。

![image](./assets/particlesystem.png)


* `ParticleSystem` - manages shared time-line between emitters （管理发射器之间的共享时间线）
* `Emitter` - emits logical particles into the system （将逻辑粒子发射到系统中）
* `ParticlePainter` - particles are visualized by a particle painter （粒子通过粒子画家来可视化）
* `Direction` - vector space for emitted particles （发射粒子的向量空间）
* `ParticleGroup` - every particle is a member of a group （每个粒子都是一个组的成员）
* `Affector` - manipulates particles after they have been emitted （在粒子发射后操控粒子）


