# Animating Shapes（动画形状）

One of the nice aspects of using Qt Quick Shapes, is that the paths drawn are defined directly in QML. This means that their properties can be bound, transitioned and animated, just like any other property in QML.

使用 Qt Quick Shapes 的一个很好的方面是，绘制的路径可以直接在 QML 中定义。这意味着它们的属性可以像 QML 中的任何其他属性一样被绑定、过渡和动画化。

![](./assets/automatic/animation.png)

In the example below, we reuse the basic shape from the very first section of this chapter, but we introduce a variable, ``t``, that we animate from ``0.0`` to ``1.0`` in a loop. We then use this variable to offset the position of the small circles, as well as the size of the top and bottom circle. This creates an animation in which it seems that the circles appear at the top and disappear towards the bottom.

在下面的示例中，我们重用了本章第一节中的基本形状，但引入了一个变量 ``t``，它在一个循环中从 ``0.0`` 动画到 ``1.0``。然后我们使用这个变量来偏移小圆的位置，以及顶部和底部圆的大小。这创建了一个动画，似乎圆从顶部出现并消失到底部。


<<< @/docs/ch09-shapes/src/shapes/animation.qml#global

Notice that instead of using a ``NumberAnimation`` on ``t``, any other binding can be used, e.g. to a slider, an external state, and so on. Your imagination is the limit.

注意，我们并没有使用 ``NumberAnimation`` 来对 ``t`` 进行动画化，任何其他的绑定都可以使用，例如一个滑块，一个外部状态等等。你的想象力是无限的。
