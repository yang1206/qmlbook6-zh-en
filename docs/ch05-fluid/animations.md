# Animations(动画)

Animations are applied to property changes. An animation defines the interpolation curve from one value to another value when a property value changes. These animation curves create smooth transitions from one value to another. 
动画应用于属性变化。动画定义了当属性值发生变化时从一个值到另一个值的插值曲线。这些动画曲线在从一个值到另一个值之间创建平滑的过渡。

An animation is defined by a series of target properties to be animated, an easing curve for the interpolation curve, and a duration. All animations in Qt Quick are controlled by the same timer and are therefore synchronized. This improves the performance and visual quality of animations.

动画由一系列要动画化的目标属性、插值曲线的缓动曲线和持续时间定义。Qt Quick中的所有动画都由相同的计时器控制，因此是同步的。这提高了动画的性能和视觉质量。



::: tip Animations control how properties change using value interpolation

动画利用数值插值控制属性的变化方式

This is a fundamental concept. QML is based on elements, properties, and scripting. Every element provides dozens of properties, each property is waiting to get animated by you. In the book, you will see this is a spectacular playing field.

这是一个基本概念。QML基于元素、属性和脚本。每个元素都提供了数十个属性，每个属性都等待着被你动画化。在本书中，你会看到这是一个令人惊叹的竞技场。

You will catch yourself looking at some animations and just admiring their beauty, and your creative genius, too. Please remember then: *animations control property changes and every element has dozens of properties at your disposal*.

你会发现自己看着一些动画，只是欣赏它们的美丽，以及你的创意天才。请记住，*动画控制属性变化，每个元素都有几十个属性供你使用*。

**Unlock the power!**
:::

![](./assets/animation_sequence.png)

<<< @/docs/ch05-fluid/src/animation/AnimationExample.qml#global

The example above shows a simple animation applied on the `x` and `rotation` properties. Each animation has a duration of 4000 milliseconds (msec). The animation on `x` moves the x-coordinate from the object gradually over to 240px. The animation on rotation runs from the current angle to 360 degrees. Both animations run in parallel and are started when the `MouseArea` is clicked.

上面的示例显示了一个简单的动画应用于`x`和`rotation`属性。每个动画的持续时间为4000毫秒（msec）。`x`上的动画逐渐将对象的x坐标移动到240px。旋转动画从当前角度运行到360度。两个动画并行运行，并在`MouseArea`被点击时启动。



You can play around with the animation by changing the `to` and `duration` properties, or you could add another animation (for example, on the `opacity` or even the `scale`). **Combining these it could look like the object is disappearing into deep space. Try it out!**

你可以通过更改`to`和`duration`属性来尝试动画，或者你可以添加另一个动画（例如，在`opacity`或甚至`scale`上）。**将这些结合起来，对象可能会消失到深空。试试看！**

## Animation Elements(动画元素)

There are several types of animation elements, each optimized for a specific use case. Here is a list of the most prominent animations:

有几种类型的动画元素，每种类型都针对特定的用例进行了优化。以下是其中最突出的动画列表：


* `PropertyAnimation` - Animates changes in property values 
* `PropertyAnimation` - 动画属性值的变化

* `NumberAnimation` - Animates changes in qreal-type values
* `NumberAnimation` - 动画`qreal`类型值的变化

* `ColorAnimation` - Animates changes in color values
* `ColorAnimation` - 动画颜色值的变化

* `RotationAnimation` - Animates changes in rotation values
* `RotationAnimation` - 动画旋转值的变化

Besides these basic and widely used animation elements, Qt Quick also provides more specialized animations for specific use cases:

除了这些基本和广泛使用的动画元素外，Qt Quick还提供了更多针对特定用例的专门动画：


* `PauseAnimation` - Provides a pause for an animation
* `PauseAnimation` - 为动画提供暂停

* `SequentialAnimation` - Allows animations to be run sequentially
* `SequentialAnimation` - 允许动画按顺序运行

* `ParallelAnimation` - Allows animations to be run in parallel
* `ParallelAnimation` - 允许动画并行运行

* `AnchorAnimation` - Animates changes in anchor values
* `AnchorAnimation` - 动画锚点值的变化

* `ParentAnimation` - Animates changes in parent values
* `ParentAnimation` - 动画父值的变化

* `SmoothedAnimation` - Allows a property to smoothly track a value
* `SmoothedAnimation` - 允许一个属性平滑地跟踪一个值

* `SpringAnimation` - Allows a property to track a value in a spring-like motion
* `SpringAnimation` - 允许一个属性以类似弹簧的方式跟踪一个值

* `PathAnimation` - Animates an item alongside a path
* `PathAnimation` - 沿着路径动画一个项目

* `Vector3dAnimation` - Animates changes in QVector3d values
* `Vector3dAnimation` - 动画`QVector3d`值的变化

Later we will learn how to create a sequence of animations. While working on more complex animations, there is sometimes a need to change a property or to run a script during an ongoing animation. For this Qt Quick offers the action elements, which can be used everywhere where the other animation elements can be used:

稍后我们将学习如何创建一系列动画。在处理更复杂的动画时，有时需要在动画过程中更改属性或运行脚本。为此，Qt Quick提供了操作元素，可以在其他动画元素可以使用的任何地方使用：


* `PropertyAction` - Specifies immediate property changes during animation
* `PropertyAction` - 在动画过程中指定立即属性更改

* `ScriptAction` - Defines scripts to be run during an animation
* `ScriptAction` - 定义在动画过程中运行的脚本

The major animation types will be discussed in this chapter using small, focused examples.

本章将通过重点突出的小例子来讨论主要的动画类型。



## Applying Animations(应用动画)

Animation can be applied in several ways:

动画可以通过几种方式应用：


* **Animation on property** - runs automatically after the element is fully loaded

* **Animation on property** - 在元素完全加载后自动运行

* **Behavior on property** - runs automatically when the property value changes
* **Behavior on property** - 当属性值更改时自动运行

* **Standalone Animation** - runs when the animation is explicitly started using `start()` or `running` is set to true (e.g. by a property binding)
* **Standalone Animation** - 使用`start()`或`running`设置为true（例如，通过属性绑定）显式启动时运行

*Later we will also see how animations can be used inside state transitions.*

*稍后我们将看到如何在状态转换中使用动画。*

## Clickable Image V2

To demonstrate the usage of animations we reuse our ClickableImage component from an earlier chapter and extended it with a text element.

为了演示动画的使用，我们重新使用我们在前面章节中使用的ClickableImage组件，并使用文本元素扩展它。

<<< @/docs/ch05-fluid/src/animation/ClickableImageV2.qml#global

To organize the element below the image we used a Column positioner and calculated the width and height based on the column’s childrenRect property. We exposed text and image source properties, and a clicked signal. We also wanted the text to be as wide as the image, and for it to wrap. We achieve the latter by using the Text element's `wrapMode` property.

为了将元素组织在图像下方，我们使用Column定位器，并根据列的childrenRect属性计算宽度和高度。我们公开了文本和图像源属性，以及一个点击信号。我们还希望文本与图像一样宽，并自动换行。我们通过使用Text元素的`wrapMode`属性来实现后者。



::: tip Parent/child geometry dependency

父/子几何依赖性

Due to the inversion of the geometry-dependency (parent geometry depends on child geometry), we can’t set a `width`/`height` on the ClickableImageV2, as this will break our `width`/`height` binding. 

由于几何依赖性的反转（父几何依赖于子几何），我们不能在ClickableImageV2上设置`width`/`height`，因为这将破坏我们的`width`/`height`绑定。

You should prefer the child’s geometry to depend on the parent’s geometry if the item is more like a container for other items and should adapt to the parent's geometry.

如果项目更像是一个其他项目的容器，并且应该适应父项的几何形状，则应优先考虑子项的几何形状。
:::

### The objects ascending(对象上升)

![](./assets/animationtypes_start.png)

The three objects are all at the same y-position (`y=200`). They all need to travel to `y=40`, each of them using a different method with different side-effects and features.

三个对象都在相同的y位置（`y=200`）。它们都需要移动到`y=40`，每个对象使用不同的方法，具有不同的副作用和功能。

### First object

The first object travels using the `Animation on <property>` strategy. The animation starts immediately. 

第一个对象使用`Animation on <property>`策略移动。动画立即开始。


<<< @/docs/ch05-fluid/src/animation/AnimationTypesExample.qml#animation-on-property

When an object is clicked, its y-position is reset to the start position, and this applies to all of the objects. On the first object, the reset does not have any effect as long as the animation is running. 

当对象被点击时，其y位置被重置为起始位置，这适用于所有对象。只要动画正在运行，重置对第一个对象没有任何影响。

This can be visually disturbing, as the y-position is set to a new value for a fraction of a second before the animation starts. *Such competing property changes should be avoided*.

这可能会引起视觉上的困扰，因为动画开始前，y位置会被设置为一个新的值。*应避免此类相互竞争的属性更改*。


### Second object

The second object travels using a `Behavior on` animation. This behavior tells the property it should animate each change in value. The behavior can be disabled by setting `enabled: false` on the `Behavior` element. 

第二个对象使用`Behavior on`动画移动。这个行为告诉属性应该动画化每个值的变化。可以通过在`Behavior`元素上设置`enabled: false`来禁用这个行为。

<<< @/docs/ch05-fluid/src/animation/AnimationTypesExample.qml#behavior-on

The object will start traveling when you click it (its y-position is then set to 40). Another click has no influence, as the position is already set. 

当你点击它时，对象会开始移动（其y位置然后被设置为40）。另一个点击没有影响，因为位置已经设置。

You could try to use a random value (e.g. `40 + (Math.random() \* (205-40)`) for the y-position. You will see that the object will always animate to the new position and adapt its speed to match the 4 seconds to the destination defined by the duration of the animation.

你可以尝试使用随机值（例如`40 + (Math.random() \* (205-40)`)作为y位置。你会看到对象总是动画到新的位置，并调整其速度以匹配动画持续时间内定义的目标位置。

### Third object

The third object uses a standalone animation. The animation is defined as its own element and can be almost anywhere in the document. 

第三个对象使用独立的动画。动画被定义为它自己的元素，可以放在文档中的任何地方。

<<< @/docs/ch05-fluid/src/animation/AnimationTypesExample.qml#standalone

The click will start the animation using the animation's `start()` function. Each animation has start(), stop(), resume(), and restart() functions. The animation itself contains much more information than the other animation types earlier. 

点击将使用动画的`start()`函数启动动画。每个动画有start(), stop(), resume(), and restart()函数，每个动画都包含比前面其他动画类型更多的信息。


We need to define the `target`, which is the element to be animated, along with the names of the properties that we want to animate. We also need to define a `to` value, and, in this case, a `from` value, which allows a restart of the animation.

我们需要定义`target`，即要动画化的元素，以及要动画化的属性的名称。我们还需要定义一个`to`值，在这个例子中还需要一个`from`值，这允许重新启动动画。


![](./assets/animationtypes.png)

A click on the background will reset all objects to their initial position. The first object cannot be restarted except by re-starting the program which triggers the re-loading of the element.

点击背景将把所有对象重置到初始位置。第一个对象不能重新启动，除非重新启动程序，这会触发元素的重新加载。


::: tip Other ways to control Animations

其他控制动画的方法

Another way to start/stop an animation is to bind a property to the `running` property of an animation. This is especially useful when the user-input is in control of properties:

另一种启动/停止动画的方法是将属性绑定到动画的`running`属性。当用户输入控制属性时，这种方法特别有用：


```qml
NumberAnimation {
    // [...]
    // animation runs when mouse is pressed
    running: area.pressed
}
MouseArea {
    id: area
}
```
:::

## Easing Curves(缓动曲线)

The value change of a property can be controlled by an animation. Easing attributes allow influencing the interpolation curve of a property change. 

属性的值变化可以通过动画控制。缓动属性允许影响属性变化的插值曲线。

All animations we have defined by now use a linear interpolation because the initial easing type of an animation is `Easing.Linear`. It’s best visualized with a small plot, where the y-axis is the property to be animated and the x-axis is the time (*duration*). A linear interpolation would draw a straight line from the `from` value at the start of the animation to the `to` value at the end of the animation. So the easing type defines the curve of change. 

到目前为止，我们定义的所有动画都使用线性插值，因为动画的初始缓动类型是`Easing.Linear`。最好通过一个小图来可视化，其中y轴是要动画化的属性，x轴是时间（*duration*）。线性插值将绘制一条从动画开始时的`from`值到动画结束时的`to`值的直线。因此，缓动类型定义了变化曲线。

Easing types should be carefully chosen to support a natural fit for a moving object. For example, when a page slides out, the page should initially slide out slowly and then gain momentum to finally slide out at high speed, similar to turning the page of a book.

缓动类型应该仔细选择，以支持移动对象的自然适应。例如，当页面滑出时，页面应该最初缓慢滑出，然后获得动力，最终以高速滑出，类似于翻书页。


:::tip Animations should not be overused. 

动画不应该过度使用。

As with other aspects of UI design, animations should be designed carefully to support the UI flow, not dominate it. The eye is very sensitive to moving objects and animations can easily distract the user.

与其他UI设计方面一样，动画应该仔细设计以支持UI流程，而不是主导它。眼睛对移动对象非常敏感，动画很容易分散用户的注意力。
:::

In the next example, we will try some easing curves. Each easing curve is displayed by a clickable image and, when clicked, will set a new easing type on the `square` animation and then trigger a `restart()` to run the animation with the new curve.

在下一个例子中，我们将尝试一些缓动曲线。每个缓动曲线都由一个可点击的图像显示，当点击时，将在`square`动画上设置一个新的缓动类型，然后触发`restart()`以使用新曲线运行动画。

![](./assets/automatic/easingcurves.png)

The code for this example was made a little bit more complicated. We first create a grid of `EasingTypes` and a `Box` which is controlled by the easing types. An easing type just displays the curve which the box shall use for its animation. When the user clicks on an easing curve the box moves in a direction according to the easing curve. The animation itself is a standalone animation with the target set to the box and configured for x-property animation with a duration of 2 seconds.

这个例子的代码稍微复杂了一些。我们首先创建了一个`EasingTypes`的网格和一个由缓动类型控制的`Box`。一个缓动类型只是显示盒子将为其动画使用的曲线。当用户点击一个缓动曲线时，盒子会根据缓动曲线向一个方向移动。动画本身是一个独立的动画，目标设置为盒子，配置为x属性动画，持续时间为2秒。

::: tip
The internals of the EasingType renders the curve in real time, and the interested reader can look it up in the `EasingCurves` example.

EasingType的内部实时渲染曲线，感兴趣的读者可以在`EasingCurves`示例中查找。
:::

<<< @/docs/ch05-fluid/src/easing/EasingCurves.qml#global

Please play with the example and observe the change of speed during an animation. Some animations feel more natural for the object and some feel irritating.

请运行这个例子并观察动画中的速度变化。一些动画对物体来说感觉更自然，而另一些则感觉令人恼火。



Besides the `duration` and `easing.type`, you are able to fine-tune animations. For example, the general `PropertyAnimation` type (from which most animations inherit) additionally supports `easing.amplitude`, `easing.overshoot`, and `easing.period` properties, which allow you to fine-tune the behavior of particular easing curves. 

除了`duration`和`easing.type`之外，你还可以微调动画。例如，一般的`PropertyAnimation`类型（大多数动画都继承自它）还支持`easing.amplitude`、`easing.overshoot`和`easing.period`属性，这些属性允许你微调特定缓动曲线的行为。

Not all easing curves support these parameters. Please consult the [easing table](http://doc.qt.io/qt-6/qml-qtquick-propertyanimation.html#easing-prop) from the `PropertyAnimation` documentation to check if an easing parameter has an influence on an easing curve.

并非所有的缓动曲线都支持这些参数。请查阅`PropertyAnimation`文档中的[缓动表](http://doc.qt.io/qt-6/qml-qtquick-propertyanimation.html#easing-prop)，以检查缓动参数是否会影响缓动曲线。

::: tip Choose the right Animation

选择正确的动画

Choosing the right animation for the element in the user interface context is crucial for the outcome. Remember the animation shall support the UI flow; not irritate the user.

在用户界面上下文中为元素选择正确的动画对于结果至关重要。请记住，动画应该支持UI流，而不是打扰用户。
:::

## Grouped Animations（分组动画）

Often animations will be more complex than just animating one property. You might want to run several animations at the same time or one after another or even execute a script between two animations. 

通常，动画会比只动画一个属性更复杂。你可能希望同时运行多个动画，或者一个接一个地运行，甚至在两个动画之间执行脚本。

For this, grouped animations can be used. As the name suggests, it’s possible to group animations. Grouping can be done in two ways: parallel or sequential. You can use the `SequentialAnimation` or the `ParallelAnimation` element, which act as animation containers for other animation elements. These grouped animations are animations themselves and can be used exactly as such.

为此，可以使用分组动画。顾名思义，可以分组动画。分组可以通过两种方式完成：并行或顺序。可以使用`SequentialAnimation`或`ParallelAnimation`元素，这些元素充当其他动画元素的动画容器。这些分组动画本身就是动画，可以像这样使用。


![](./assets/groupedanimation.png)

### Parallel animations(并行动画)

All direct child animations of a parallel animation run in parallel when started. This allows you to animate different properties at the same time.

当启动并行动画时，并行动画的所有直接子动画同时运行。这允许你同时动画不同的属性。

<<< @/docs/ch05-fluid/src/animation/ParallelAnimationExample.qml#global

![](./assets/parallelanimation_sequence.png)

### Sequential animations(顺序动画)

A sequential animation runs each child animation in the order in which it is declared: top to bottom.

顺序动画按照声明的顺序运行每个子动画：从上到下。

<<< @/docs/ch05-fluid/src/animation/SequentialAnimationExample.qml#global

![](./assets/sequentialanimation_sequence.png)

### Nested animations(嵌套动画)

Grouped animations can also be nested. For example, a sequential animation can have two parallel animations as child animations, and so on. We can visualize this with a soccer ball example. The idea is to throw a ball from left to right and animate its behavior.

分组动画也可以嵌套。例如，顺序动画可以有两组并行动画作为子动画，依此类推。我们可以用一个足球例子来可视化这个。想法是从左到右抛球，并动画其行为。

![](./assets/soccer_init.png)

To understand the animation we need to dissect it into the integral transformations of the object. We need to remember that animations animate property changes. Here are the different transformations:

要理解动画，我们需要将其分解为对象的积分变换。我们需要记住，动画会动画属性变化。以下是不同的变换：


* An x-translation from left-to-right (`X1`)

* A y-translation from bottom to top (`Y1`) followed by a translation from up to down (`Y2`) with some bouncing

* A rotation of 360 degrees over the entire duration of the animation (`ROT1`)

The whole duration of the animation should take three seconds.

整个动画时间应为三秒钟。

![](./assets/soccer_plan.png)

We start with an empty item as the root element of the width of 480 and height of 300.

我们从宽度为480，高度为300的`Item`开始，作为根元素。


```qml
import QtQuick

Item {
    id: root

    property int duration: 3000

    width: 480
    height: 300

    // [...]
}
```

We have defined our total animation duration as a reference to better synchronize the animation parts.

我们定义了我们的总动画时间作为参考，以更好地同步动画部分。


The next step is to add the background, which in our case are 2 rectangles with green and blue gradients.

下一步是添加背景，在我们的例子中是2个带有绿色和蓝色渐变的矩形。

<<< @/docs/ch05-fluid/src/animation/BouncingBallExample.qml#background

![](./assets/soccer_stage1.png)

The upper blue rectangle takes 200 pixels of the height and the lower one is anchored to the bottom of the sky and to the bottom of the root element.

上面的蓝色矩形占用200像素的高度，而下面的一个则锚定在天空的底部和根元素的底部。


Let’s bring the soccer ball onto the green. The ball is an image, stored under “assets/soccer_ball.png”. For the beginning, we would like to position it in the lower left corner, near the edge.

让我们把足球放到绿色上。球是一个图片，存储在“assets/soccer_ball.png”下。一开始，我们想把它放在左下角，靠近边缘。


<<< @/docs/ch05-fluid/src/animation/BouncingBallExample.qml#soccer-ball

![](./assets/soccer_stage2.png)

The image has a mouse area attached to it. If the ball is clicked, the position of the ball will reset and the animation is restarted.

图片上附有一个鼠标区域。如果点击球，球的位置将重置，动画将重新启动。

Let’s start with a sequential animation for the two y translations first.

首先，让我们用两个y平移的顺序动画开始。

```qml
SequentialAnimation {
    id: anim
    NumberAnimation {
        target: ball
        properties: "y"
        to: 20
        duration: root.duration * 0.4
    }
    NumberAnimation {
        target: ball
        properties: "y"
        to: 240
        duration: root.duration * 0.6
    }
}
```

![](./assets/soccer_stage3.png)

This specifies that 40% of the total animation duration is the up animation and 60% the down animation, with each animation running after the other in sequence. The transformations are animated on a linear path but there is no curving currently. Curves will be added later using the easing curves, at the moment we’re concentrating on getting the transformations animated.

这指定了总动画时间的40%是向上动画，60%是向下动画，每个动画在序列中依次运行。变换是线性路径上动画化的，但目前没有曲线。曲线稍后使用缓动曲线添加，目前我们专注于使变换动画化。

Next, we need to add the x-translation. The x-translation shall run in parallel with the y-translation, so we need to encapsulate the sequence of y-translations into a parallel animation together with the x-translation.

接下来，我们需要添加x平移。x平移应该与y平移并行运行，所以我们需要将y平移的序列封装到与x平移并行的并行动画中。


```qml
ParallelAnimation {
    id: anim
    SequentialAnimation {
        // ... our Y1, Y2 animation
    }
    NumberAnimation { // X1 animation
        target: ball
        properties: "x"
        to: 400
        duration: root.duration
    }
}
```

![](./assets/soccer_stage4.png)

In the end, we would like the ball to be rotating. For this, we need to add another animation to the parallel animation. We choose `RotationAnimation`, as it’s specialized for rotation.

最后，我们希望球在旋转。为此，我们需要在并行动画中添加另一个动画。我们选择`RotationAnimation`，因为它专门用于旋转。


```qml
ParallelAnimation {
    id: anim
    SequentialAnimation {
        // ... our Y1, Y2 animation
    }
    NumberAnimation { // X1 animation
        // X1 animation
    }
    RotationAnimation {
        target: ball
        properties: "rotation"
        to: 720
        duration: root.duration
    }
}
```

That’s the whole animation sequence. The one thing that's left is to provide the correct easing curves for the movements of the ball. For the *Y1* animation, we use a `Easing.OutCirc` curve, as this should look more like a circular movement. *Y2* is enhanced using an `Easing.OutBounce` to give the ball its bounce, and the bouncing should happen at the end (try with `Easing.InBounce` and you will see that the bouncing starts right away).

这就是整个动画序列。剩下的就是为球的运动提供正确的缓动曲线。对于*Y1*动画，我们使用`Easing.OutCirc`曲线，因为这将看起来更像是一个圆形运动。*Y2*使用`Easing.OutBounce`增强，以给球带来弹跳，弹跳应该发生在最后（尝试使用`Easing.InBounce`，你会看到弹跳会立即开始）。


The *X1* and *ROT1* animation are left as-is, with a linear curve.

*X1*和*ROT1*动画保持原样，使用线性曲线。


Here is the final animation code for your reference:

以下是最终的动画代码，供您参考：

<<< @/docs/ch05-fluid/src/animation/BouncingBallExample.qml#parallel-animation
