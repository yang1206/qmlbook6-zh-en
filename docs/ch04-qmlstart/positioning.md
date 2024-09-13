# Positioning Elements(定位元素)

There are a number of QML elements used to position items. These are called positioners, of which the Qt Quick module provides the following: `Row`, `Column`, `Grid` and `Flow`. They can be seen showing the same contents in the illustration below.

QML 提供了几种用于定位项的元素，这些被称为定位器。Qt Quick 模块提供了以下定位器：`Row`、`Column`、`Grid` 和 `Flow`。它们在下面的图中显示了相同的内容。


::: tip
Before we go into details, let me introduce some helper elements: the red, blue, green, lighter and darker squares. Each of these components contains a 48x48 pixel colorized rectangle. As a reference, here is the source code for the `RedSquare`:

在详细介绍之前，让我介绍一些辅助元素：红色、蓝色、绿色、浅色和深色的正方形。每个这些组件都包含一个48x48像素的着色矩形。作为参考，这里是 `RedSquare` 的源代码：
:::

<<< @/docs/ch04-qmlstart/src/positioners/RedSquare.qml#global

Please note the use of `Qt.lighter(color)` to produce a lighter border color based on the fill color. We will use these helpers in the next examples to make the source code more compact and readable. Please remember, each rectangle is initially 48x48 pixels.

请注意使用 `Qt.lighter(color)` 来根据填充颜色产生一个较浅的边框颜色。在下一个示例中，我们将使用这些辅助元素来使源代码更紧凑和易读。请记住，每个矩形最初都是48x48像素。

The `Column` element arranges child items into a column by stacking them on top of each other. The `spacing` property can be used to distance each of the child elements from each other.

`Column` 元素通过将子项堆叠在一起来将子项排列成一列。可以使用 `spacing` 属性来将每个子元素彼此分开。

![](./assets/column.png)

<<< @/docs/ch04-qmlstart/src/positioners/ColumnExample.qml#global

The `Row` element places its child items next to each other, either from the left to the right, or from the right to the left, depending on the `layoutDirection` property. Again, `spacing` is used to separate child items.

`Row` 元素将它的子项并排放置，要么从左到右，要么从右到左，取决于 `layoutDirection` 属性。同样，`spacing` 用于分隔子项。

![](./assets/row.png)

<<< @/docs/ch04-qmlstart/src/positioners/RowExample.qml#global

The `Grid` element arranges its children in a grid. By setting the `rows` and `columns` properties, the number of rows or columns can be constrained. By not setting either of them, the other is calculated from the number of child items. For instance, setting rows to 3 and adding 6 child items will result in 2 columns. The properties `flow` and `layoutDirection` are used to control the order in which the items are added to the grid, while `spacing` controls the amount of space separating the child items.

`Grid` 元素将其子项排列成一个网格。通过设置 `rows` 和 `columns` 属性，可以限制行数或列数。如果不设置其中任何一个，则根据子项数量计算另一个。例如，将行设置为3，并添加6个子项，将导致2列。属性 `flow` 和 `layoutDirection` 用于控制将子项添加到网格中的顺序，而 `spacing` 控制子项之间的空间量。

![](./assets/grid.png)

<<< @/docs/ch04-qmlstart/src/positioners/GridExample.qml#global

The final positioner is `Flow`. It adds its child items in a flow. The direction of the flow is controlled using `flow` and `layoutDirection`. It can run sideways or from the top to the bottom. It can also run from left to right or in the opposite direction. As the items are added in the flow, they are wrapped to form new rows or columns as needed. In order for a flow to work, it must have a width or a height. This can be set either directly, or though anchor layouts.

`Flow` 是最后一个定位器。它在其子项中添加流。流的方向由 `flow` 和 `layoutDirection` 控制。它可以水平运行，也可以从上到下运行。它也可以从左到右运行，或者在相反的方向上运行。随着项目在流中添加，它们会根据需要换行或换列。为了使流工作，它必须有一个宽度和高度。可以直接设置，也可以通过锚定位器设置。

![](./assets/flow.png)

<<< @/docs/ch04-qmlstart/src/positioners/FlowExample.qml#global

An element often used with positioners is the `Repeater`. It works like a for-loop and iterates over a model. In the simplest case a model is just a value providing the number of loops.

`Repeater` 是与定位器一起使用的元素。它的工作方式像一个 for 循环，遍历一个模型。在最简单的情况下，模型只是一个提供循环次数的值。

![](./assets/repeater.png)

<<< @/docs/ch04-qmlstart/src/positioners/RepeaterExample.qml#global

In this repeater example, we use some new magic. We define our own `colorArray` property, which is an array of colors. The repeater creates a series of rectangles (16, as defined by the model). For each loop, it creates the rectangle as defined by the child of the repeater. In the rectangle we chose the color by using JS math functions: `Math.floor(Math.random()*3)`. This gives us a random number in the range from 0..2, which we use to select the color from our color array. As noted earlier, JavaScript is a core part of Qt Quick, and as such, the standard libraries are available to us.

在这个重复器示例中，我们使用了一些新的魔法。我们定义了自己的 `colorArray` 属性，它是一个颜色数组。重复器创建了一系列矩形（16个，如模型中定义的）。对于每次循环，它根据重复器的子项创建矩形。在矩形中，我们通过使用 JS 数学函数 `Math.floor(Math.random()*3)` 来选择颜色。这给我们一个0..2范围内的随机数，我们用它从颜色数组中选择颜色。如前所述，JavaScript 是 Qt Quick 的核心部分，因此，我们可以使用标准库。

A repeater injects the `index` property into the repeater. It contains the current loop-index. (0,1,..15). We can use this to make our own decisions based on the index, or in our case to visualize the current index with the `Text` element. 

重复器将 `index` 属性注入到重复器中。它包含当前的循环索引。（0,1,..15）。我们可以使用它来根据索引做出自己的决定，或者在我们的情况下，使用 `Text` 元素来可视化当前的索引。

::: tip
While the `index` property is dynamically injected into the Rectangle, it is a good practice to declare it as a required property to ease readability and help tooling. This is achieved by the `required property int index` line. 

虽然 `index` 属性会动态地注入到矩形中，但将其声明为必需属性以简化可读性和帮助工具是一个好习惯。这是通过 `required property int index` 行实现的。
:::

::: tip
More advanced handling of larger models and kinetic views with dynamic delegates is covered in its own model-view chapter. Repeaters are best used when having a small amount of static data to be presented.

更高级的处理较大模型和动态代理的动态视图将在其自己的模型视图章节中介绍。重复器最适合用于要呈现的小量静态数据。
:::

