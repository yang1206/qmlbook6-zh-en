# States and Transitions(状态和过渡)

Often parts of a user interface can be described in states. A state defines a set of property changes and can be triggered by a certain condition. 

用户界面的某些部分通常可以用状态来描述。状态定义了一组属性变化，并可由特定条件触发。

Additionally, these state switches can have a transition attached which defines how these changes should be animated or any additional actions that shall be applied. Actions can also be applied when a state is entered.

此外，这些状态切换可以附加过渡，该过渡定义了这些变化应如何动画化，或应用任何其他附加操作。当进入状态时，也可以应用操作。


## States

You define states in QML with the `State` element, which needs to be bound to the `states` array of any item element. 

您可以使用 `State` 元素在 QML 中定义状态，该元素需要绑定到任何项元素的 `states` 数组。

A state is identified through a state name, and in its simplest form, consists of a series of property changes on elements. The default state is defined by the initial properties of the element and is named `""` (an empty string).

状态通过状态名称进行标识，在最简单的情况下，由一系列元素属性更改组成。默认状态由元素的初始属性定义，并命名为 `""`（空字符串）。


```qml
Item {
    id: root
    states: [
        State {
            name: "go"
            PropertyChanges { ... }
        },
        State {
            name: "stop"
            PropertyChanges { ... }
        }
    ]
}
```

A state is changed by assigning a new state name to the `state` property of the element in which the states are defined.

通过将新状态名称分配给定义状态的元素的 `state` 属性，可以更改状态。


::: tip Control states using when

使用 `when` 属性控制状态

Another way to control states is using the `when` property of the `State` element. The `when` property can be set to an expression that evaluates to true when the state should be applied.

另一种控制状态的方法是使用 `State` 元素的 `when` 属性。当应用状态时，`when` 属性可以设置为评估为 `true` 的表达式。
:::

```qml
Item {
    id: root
    states: [
        ...
    ]

    Button {
        id: goButton
        ...
        onClicked: root.state = "go"
    }
}
```

![](./assets/trafficlight_sketch.png)

For example, a traffic light might have two signaling lights. The upper one signaling stop with a red color and the lower one signaling go with a green color. In this example, both lights should not shine at the same time. Let’s have a look at the state chart diagram.

例如，交通信号灯可能有两个信号灯。上面的一个用红色表示停止，下面的一个用绿色表示前进。在这个例子中，两个灯不应该同时亮起。让我们看一下状态图。


![](./assets/trafficlight_states.png)

When the system is switched on, it automatically goes into the stop mode as the default state. The stop state changes `light1` to red and `light2` to black (off). 

当系统启动时，它自动进入停止模式，因为这是默认状态。停止状态将 `light1` 更改为红色，将 `light2` 更改为黑色（关闭）。

An external event can now trigger a state switch to the `"go"` state. In the go state, we change the color properties from `light1` to black (off) and `light2` to green to indicate the pedestrians may now cross.

现在，外部事件可以触发状态切换到 `"go"` 状态。在 go 状态中，我们将 `light1` 的颜色属性更改为黑色（关闭），将 `light2` 的颜色属性更改为绿色，以指示行人现在可以过马路。


To realize this scenario we start sketching our user interface for the 2 lights. For simplicity, we use 2 rectangles with the radius set to the half of the width (and the width is the same as the height, which means it’s a square).

要实现这个场景，我们首先为 2 个灯绘制用户界面。为了简单起见，我们使用 2 个矩形，将半径设置为宽度的半（宽度与高度相同，这意味着它是一个正方形）。


<<< @/docs/ch05-fluid/src/animation/StatesExample.qml#lights

As defined in the state chart we want to have two states: one being the `"go"` state and the other the `"stop"` state, where each of them changes the traffic light's respective color to red or green. We set the `state` property to `stop` to ensure the initial state of our traffic light is the `stop` state.

根据状态图，我们希望有两种状态：一种是 `"go"` 状态，另一种是 `"stop"` 状态，每种状态都会将交通灯的颜色更改为红色或绿色。我们将 `state` 属性设置为 `stop`，以确保交通灯的初始状态是 `stop` 状态。

::: tip Initial state

初始化状态

We could have achieved the same effect with only a `"go"` state and no explicit `"stop"` state by setting the color of `light1` to red and the color of `light2` to black. The initial state `""` defined by the initial property values would then act as the `"stop"` state.

我们可以通过将 `light1` 的颜色设置为红色，将 `light2` 的颜色设置为黑色，只使用一个 `"go"` 状态，而不显式定义 `"stop"` 状态来实现相同的效果。然后，由初始属性值定义的初始状态将作为 `"stop"` 状态。
:::

<<< @/docs/ch05-fluid/src/animation/StatesExample.qml#states

Using `PropertyChanges { target: light2; color: "black" }` is not really required in these examples as the initial color of `light2` is already black. In a state, it’s only necessary to describe how the properties shall change from their default state (and not from the previous state).

在这些示例中，使用 `PropertyChanges { target: light2; color: "black" }` 并不是必需的，因为 `light2` 的初始颜色已经是黑色。在状态中，只需要描述属性应该如何从其默认状态（而不是从前一个状态）改变。

A state change is triggered using a mouse area which covers the whole traffic light and toggles between the go- and stop-state when clicked.

使用覆盖整个交通灯的鼠标区域来触发状态切换，并在单击时在 go- 和 stop 状态之间切换。

<<< @/docs/ch05-fluid/src/animation/StatesExample.qml#interaction

![](./assets/trafficlight_ui.png)

We are now able to successfully change the state of the traffic lamp. To make the UI more appealing and natural, we should add some transitions with animation effects. A transition can be triggered by a state change.

我们现在能够成功改变交通灯的状态。为了使 UI 更具吸引力和自然，我们应该添加一些带有动画效果的过渡。过渡可以通过状态变化来触发。


::: tip Using scripting

使用脚本

It’s possible to create similar logic using scripting instead of QML states. However, QML is a better language than JavaScript for describing user interfaces. Where possible, aim to write declarative code instead of imperative code.

可以使用脚本而不是 QML 状态来创建类似的逻辑。然而，QML 是一种比 JavaScript 更适合描述用户界面的语言。尽可能编写声明式代码而不是命令式代码。
:::


## Transitions(过渡)

A series of transitions can be added to every item. A transition is executed by a state change.

可以为每个项目添加一系列过渡。过渡由状态变化执行。


You can define on which state change a particular transition can be applied using the `from:` and `to:` properties. These two properties act like a filter: when the filter is true the transition will be applied. You can also use the wildcard “\*”, which means “any state”. 

你可以使用 `from:` 和 `to:` 属性来定义特定过渡可以在哪个状态变化上应用。这两个属性充当过滤器：当过滤器为真时，将应用过渡。你也可以使用通配符“\*”，这意味着“任何状态”。

For example, `from: "*"; to: "*"` means "from any state to any other state", and is the default value for `from` and `to`. This means the transition will be applied to every state switch.

例如，`from: "*"; to: "*"` 表示“从任何状态到任何其他状态”，是 `from` 和 `to` 的默认值。这意味着过渡将应用于每个状态切换。

For this example, we would like to animate the color changes when switching state from “go” to “stop”. For the other reversed state change (“stop” to “go”) we want to keep an immediate color change and don’t apply a transition. 

对于这个示例，我们希望在从“go”状态切换到“stop”状态时对颜色变化进行动画处理。对于相反的状态变化（“stop”到“go”），我们希望保持立即的颜色变化，并且不应用过渡。

We restrict the transition with the `from` and `to` properties to filter only the state change from “go” to “stop”. Inside the transition, we add two color animations for each light, which shall animate the property changes defined in the state description.

我们使用 `from` 和 `to` 属性限制过渡，以过滤只有从“go”到“stop”的状态变化。在过渡内部，我们为每个灯添加两个颜色动画，这些动画将动画化在状态描述中定义的属性变化。

<<< @/docs/ch05-fluid/src/animation/StatesExample.qml#transitions

You can change the state though clicking the UI. The state is applied immediately and will also change the state while a transition is running. So, try to click the UI while the state is in the transition from “stop” to “go”. You will see the change will happen immediately.

你可以通过点击 UI 来改变状态。状态会立即应用，并且在过渡期间也会改变状态。所以，尝试在状态从“stop”到“go”的过渡期间点击 UI。你会看到变化会立即发生。


![](./assets/trafficlight_transition.png)

You could play around with this UI by, for example, scaling the inactive light down to highlight the active light. 

你可以通过，例如，将不活动的灯缩小以突出显示活动的灯，来尝试这个 UI。

For this, you would need to add another property change for scaling to the states and also handle the animation for the scaling property in the transition. 

为此，你需要在状态中添加另一个缩放属性变化，并在过渡中处理缩放属性的动画。

Another option would be to add an “attention” state where the lights are blinking yellow. For this, you would need to add a sequential animation to the transition for one second going to yellow (“to” property of the animation and one second going to “black”).

另一个选项是添加一个“注意”状态，其中灯光闪烁黄色。为此，你需要在过渡中添加一个一秒钟到黄色的顺序动画（动画的“to”属性和一秒钟到“黑色”）。

Maybe you would also want to change the easing curve to make it more visually appealing.

也许你也会想改变缓动曲线，使其更具视觉吸引力。

