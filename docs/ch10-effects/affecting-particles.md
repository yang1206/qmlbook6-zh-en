# Affecting Particles(影响粒子)

Particles are emitted by the emitter. After a particle was emitted it can’t be changed any more by the emitter. The affectors allows you to influence particles after they have been emitted.

粒子由发射器发射。发射后，粒子不能由发射器更改。影响器允许您在发射后影响粒子。


Each type of affector affects particles in a different way:

每种影响器都以不同的方式影响粒子：

* `Age` - 改变粒子的生命周期
* `Attractor` - 吸引粒子向特定点移动
* `Friction` - 按照粒子的当前速度比例减速
* `Gravity` - 设置一个角度的加速度
* `Turbulence` - 基于噪声图像的流体式力
* `Wander` - 随机改变轨迹
* `GroupGoal` - 改变一组粒子的状态
* `SpriteGoal` - 改变精灵粒子的状态


* `Age` - alter where the particle is in its life-cycle
* `Attractor` - attract particles towards a specific point
* `Friction` - slows down movement proportional to the particle’s current velocity
* `Gravity` - set’s an acceleration in an angle
* `Turbulence` - fluid like forces based on a noise image
* `Wander` -  randomly vary the trajectory
* `GroupGoal` -  change the state of a group of a particle
* `SpriteGoal` - change the state of a sprite particle

## Age（年龄）

Allows particle to age faster. the `lifeLeft` property specified how much life a particle should have left.

允许粒子更快地老化。`lifeLeft`属性指定粒子应该剩下多少生命。


<<< @/docs/ch10-effects/src/particles/age.qml#M1

In the example, we shorten the life of the upper particles once when they reach the age of affector to 1200 msec. As we have set the `advancePosition` to true, we see the particle appearing again on a position when the particle has 1200 msecs left to live.

在示例中，当粒子达到1200毫秒的年龄时，我们缩短了上方的粒子的寿命。由于我们将`advancePosition`设置为true，因此当粒子剩下1200毫秒的生命时，我们看到粒子再次出现在一个位置上。

![image](./assets/age.png)

## Attractor（吸引器）

The attractor attracts particles towards a specific point. The point is specified using `pointX` and `pointY`, which is relative to the attractor geometry. The strength specifies the force of attraction. In our example we let particles travel from left to right. The attractor is placed on the top and half of the particles travel through the attractor. Affector only affect particles while they are in their bounding box. This split allows us to see the normal stream and the affected stream simultaneous.

吸引器将粒子吸引到特定点。该点使用`pointX`和`pointY`指定，该点相对于吸引器几何体。强度指定了吸引力。在我们的示例中，我们让粒子从左向右移动。吸引器位于顶部，一半的粒子穿过吸引器。影响器只影响在其边界框内的粒子。这种划分允许我们同时看到正常的流和受影响的流。


<<< @/docs/ch10-effects/src/particles/attractor.qml#M1

It’s easy to see that the upper half of the particles are affected by the attracted to the top. The attraction point is set to top-left (0/0 point) of the attractor with a force of 1.0.

很容易看到上半部分的粒子受到吸引到顶部的吸引。吸引力点设置为吸引器的左上角（0/0点）的力为1.0。

![image](./assets/attractor.png)

## Friction（摩擦）

The friction affector slows down particles by a factor until a certain threshold is reached.

摩擦影响器通过一定比例减慢粒子，直到达到某个阈值。

<<< @/docs/ch10-effects/src/particles/friction.qml#M1

In the upper friction area, the particles are slowed down by a factor of 0.8 until the particle reaches 25 pixels per seconds velocity. The threshold act’s like a filter. Particles traveling above the threshold velocity are slowed down by the given factor.

在上面的摩擦区域，粒子通过0.8的因子减慢，直到粒子达到每秒25像素的速度。阈值起过滤器的作用。速度超过阈值的粒子会以给定的因子减慢。

![image](./assets/friction.png)

## Gravity（重力）

The gravity affector applies an acceleration In the example we stream the particles from the bottom to the top using an angle direction. The right side is unaffected, where on the left a gravity effect is applied. The gravity is angled to 90 degrees (bottom-direction) with a magnitude of 50.

重力影响器施加一个加速度。在示例中，我们使用角度方向从底部流到顶部。右侧不受影响，而在左侧应用了重力效果。重力角度为90度（底部方向），大小为50。

<<< @/docs/ch10-effects/src/particles/gravity.qml#M1

Particles on the left side try to climb up, but the steady applied acceleration towards the bottom drags them into the direction of the gravity.

左侧的粒子试图爬升，但持续施加的向底部方向的加速度将它们拖向重力方向。

![image](./assets/gravity.png)

## Turbulence（湍流）

The turbulence affector applies a *chaos* map of force vectors to the particles. The chaos map is defined by a noise image, which can be defined with the *noiseSource* property. The strength defines how strong the vector will be applied to the particle movements.

湍流影响器将力向量图映射到粒子上。混乱图由噪声图像定义，可以使用`noiseSource`属性定义。强度定义了向量将有多大的强度应用于粒子运动。


<<< @/docs/ch10-effects/src/particles/turbulence.qml#M1

In the upper area of the example, particles are influenced by the turbulence. Their movement is more erratic. The amount of erratic deviation from the original path is defined by the strength.

在示例的上部区域，粒子受到湍流的影响。它们的运动更加不稳定。偏离原始路径的不稳定程度由强度定义。

![image](./assets/turbulence.png)

## Wander（徘徊）

The wander manipulates the trajectory. With the property *affectedParameter* can be specified which parameter (velocity, position or acceleration) is affector by the wander. The *pace* property specifies the maximum of attribute changes per second. The yVariance and yVariance specify the influence on x and y component of the particle trajectory.

徘徊操纵轨迹。使用属性`affectedParameter`可以指定哪个参数（速度、位置或加速度）受到徘徊的影响。`pace`属性指定每秒属性变化的最大值。`yVariance`和`yVariance`指定对粒子轨迹的x和y分量的影响。

<<< @/docs/ch10-effects/src/particles/wander.qml#M1

In the top wander affector particles are shuffled around by random trajectory changes. In this case, the position is changed 200 times per second in the y-direction.

在上面的徘徊影响器中，粒子通过随机轨迹变化而随机移动。在这种情况下，每秒在y方向上改变位置200次。

![image](./assets/wander.png)
