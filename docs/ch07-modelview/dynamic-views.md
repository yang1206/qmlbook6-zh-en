# Dynamic Views(动态视图)

Repeaters work well for limited and static sets of data, but in the real world, models are commonly more complex – and larger. Here, a smarter solution is needed. For this, Qt Quick provides the `ListView` and `GridView` elements. These are both based on a `Flickable` area, so the user can move around in a larger dataset. At the same time, they limit the number of concurrently instantiated delegates. For a large model, that means fewer elements in the scene at once.


Repeater适用于有限和静态的数据集，但在现实世界中，模型通常更复杂——更大。为此，Qt Quick提供了`ListView`和`GridView`元素。这两个元素都基于Flickable区域，因此用户可以在更大的数据集中移动。同时，它们限制了同时实例化的代理数量。对于大型模型，这意味着场景中同时存在的元素更少。


![image](./assets/automatic/gridview-basic.png)

The two elements are similar in their usage. We will begin with the `ListView` and then describe the `GridView` with the former as the starting point of the comparison. Notice that the `GridView` places a list of items into a two-dimensional grid, left-to-right or top-to-bottom. If you want to show a table of data you need to use the `TableView` which is described in the Table Models section.

两个元素的使用方法相似。我们将从`ListView`开始，然后用前者作为比较的起点。请注意，`GridView`将一个项目列表放置在一个二维网格中，从左到右或从上到下。如果你想显示一个数据表，你需要使用在Table Models部分描述的`TableView`。

The `ListView` is similar to the `Repeater` element. It uses a `model`, instantiates a `delegate` and between the delegates, there can be `spacing`. The listing below shows how a simple setup can look.

`ListView`与`Repeater`元素类似。它使用一个`model`，实例化一个`delegate`，在代理之间可以有`spacing`。下面的列表显示了如何进行简单的设置。


<<< @/docs/ch07-modelview/src/listview/basic.qml#global

![image](./assets/automatic/listview-basic.png)

If the model contains more data than can fit onto the screen, the `ListView` only shows part of the list. However, as a consequence of the default behavior of Qt Quick, the list view does not limit the screen area within which the delegates are shown. This means that delegates may be visible outside the list view and that the dynamic creation and destruction of delegates outside the list view is visible to the user. To prevent this, clipping must be activated on the `ListView` element by setting the `clip` property to `true`. The illustration below shows the result of this (left view), compared to when the `clip` property is `false` (right view).

如果模型包含的数据比屏幕上能容纳的要多，`ListView`只会显示列表的一部分。然而，由于Qt Quick的默认行为，列表视图并没有限制显示代理的屏幕区域。这意味着代理可能可见于列表视图之外，并且列表视图之外的代理的动态创建和销毁对用户是可见的。为了防止这种情况，必须在`ListView`元素上激活剪裁，通过将`clip`属性设置为`true`。下图显示了结果（左视图），与`clip`属性为`false`（右视图）相比。

![image](./assets/automatic/listview-clip.png)

To the user, the `ListView` is a scrollable area. It supports kinetic scrolling, which means that it can be flicked to quickly move through the contents. By default, it also can be stretched beyond the end of contents, and then bounces back, to signal to the user that the end has been reached.

对用户而言，`ListView`是一个可滚动的区域。它支持动能滚动，这意味着可以通过快速轻扫来迅速浏览内容。默认情况下，它还可以在内容末端之外进行拉伸，然后弹回，以此向用户信号表明已到达内容的末端。



The behavior at the end of the view is controlled using the `boundsBehavior` property. This is an enumerated value and can be conimaged from the default behavior, `Flickable.DragAndOvershootBounds`, where the view can be both dragged and flicked outside its boundaries, to `Flickable.StopAtBounds`, where the view never will move outside its boundaries. The middle ground, `Flickable.DragOverBounds` lets the user drag the view outside its boundaries, but flicks will stop at the boundary.


视图末端的行为是通过`boundsBehavior`属性来控制的。这是一个枚举值，可以从默认行为`Flickable.DragAndOvershootBounds`（其中视图既可以被拖动也可以被轻扫至边界之外）调整为`Flickable.StopAtBounds`（其中视图永远不会移出其边界）。介于两者之间的是`Flickable.DragOverBounds`，它允许用户将视图拖动到边界之外，但轻扫会在边界处停止。




It is possible to limit the positions where a view is allowed to stop. This is controlled using the `snapMode` property. The default behavior, `ListView.NoSnap`, lets the view stop at any position. By setting the `snapMode` property to `ListView.SnapToItem`, the view will always align the top of an item with its top. Finally, the `ListView.SnapOneItem`, the view will stop no more than one item from the first visible item when the mouse button or touch was released. The last mode is very handy when flipping through pages.

有可能限制视图允许停止的位置。这是通过`snapMode`属性来控制的。默认行为`ListView.NoSnap`允许视图在任何位置停止。通过将`snapMode`属性设置为`ListView.SnapToItem`，视图将始终将项目的顶部与顶部对齐。最后，`ListView.SnapOneItem`，视图将在鼠标按钮或触摸释放时，与第一个可见项目最多停止一个项目。最后一种模式在浏览页面时非常方便。

## Orientation(方向)

The list view provides a vertically scrolling list by default, but horizontal scrolling can be just as useful. The direction of the list view is controlled through the `orientation` property. It can be set to either the default value, `ListView.Vertical`, or to `ListView.Horizontal`. A horizontal list view is shown below.

默认情况下，列表视图提供垂直滚动的列表，但水平滚动也可能同样有用。列表视图的方向通过`orientation`属性控制。它可以设置为默认值`ListView.Vertical`或`ListView.Horizontal`。下面的示例展示了水平列表视图。

<<< @/docs/ch07-modelview/src/listview/horizontal.qml#global

![image](./assets/automatic/listview-horizontal.png)

As you can tell, the direction of the horizontal flows from the left to the right by default. This can be controlled through the `layoutDirection` property, which can be set to either `Qt.LeftToRight` or `Qt.RightToLeft`, depending on the flow direction.

如你所见，默认情况下，水平方向从左到右流动。这可以通过`layoutDirection`属性来控制，它可以设置为`Qt.LeftToRight`或`Qt.RightToLeft`，具体取决于流的方向。


## Keyboard Navigation and Highlighting(键盘导航和高亮)

When using a `ListView` in a touch-based setting, the view itself is enough. In a scenario with a keyboard, or even just arrow keys to select an item, a mechanism to indicate the current item is needed. In QML, this is called highlighting.

在触摸式设置中使用`ListView`时，视图本身就足够了。在键盘场景中，或者甚至只是使用箭头键选择一个项目，需要一个机制来指示当前项目。在QML中，这被称为高亮显示。

Views support a highlight delegate which is shown in the view together with the delegates. It can be considered an additional delegate, only that it is only instantiated once, and is moved into the same position as the current item.

视图支持一个高亮代理，它和代理一起显示在视图中。它可以被认为是一个额外的代理，只是它只实例化一次，并且被移动到与当前项目相同的位置。

In the example below this is demonstrated. There are two properties involved for this to work. First, the `focus` property is set to true. This gives the `ListView` the keyboard focus. Second, the `highlight` property is set to point out the highlighting delegate to use. The highlight delegate is given the `x`, `y` and `height` of the current item. If the `width` is not specified, the width of the current item is also used.

下面的示例展示了这一点。有两个属性与此相关。首先，`focus`属性被设置为true。这给`ListView`键盘焦点。其次，`highlight`属性被设置为指向要使用的高亮代理。高亮代理被赋予当前项目的`x`、`y`和`height`。如果未指定`width`，则使用当前项目的宽度。


In the example, the `ListView.view.width` attached property is used for width. The attached properties available to delegates are discussed further in the delegate section of this chapter, but it is good to know that the same properties are available to highlight delegates as well.

在示例中，`ListView.view.width`附加属性被用于宽度。本章的代理部分将更详细地讨论可用的附加属性，但重要的是要知道，相同的属性也可以用于高亮代理。

<<< @/docs/ch07-modelview/src/listview/highlight.qml#global

![image](./assets/automatic/listview-highlight.png)

When using a highlight in conjunction with a `ListView`, a number of properties can be used to control its behavior. The `highlightRangeMode` controls how the highlight is affected by what is shown in the view. The default setting, `ListView.NoHighlightRange` means that the highlight and the visible range of items in the view not being related at all.

当将高亮与`ListView`一起使用时，可以使用许多属性来控制其行为。`highlightRangeMode`控制高亮如何受到视图中所显示内容的影响。默认设置`ListView.NoHighlightRange`意味着高亮和视图中所显示的项目范围之间没有任何关系。


The value `ListView.StrictlyEnforceRange` ensures that the highlight is always visible. If an action attempts to move the highlight outside the visible part of the view, the current item will change accordingly, so that the highlight remains visible.

值`ListView.StrictlyEnforceRange`确保高亮总是可见的。如果某个操作试图将高亮移动到视图的可见部分之外，当前项目将相应地改变，以便高亮保持可见。

The middle ground is the `ListView.ApplyRange` value. It attempts to keep the highlight visible but does not alter the current item to enforce this. Instead, the highlight is allowed to move out of view if necessary.

中间立场是`ListView.ApplyRange`值。它试图保持高亮可见，但不改变当前项目来强制执行此操作。相反，如果需要，允许高亮移动出视图。

In the default configuration, the view is responsible for moving the highlight into position. The speed of the movement and resizing can be controlled, either as a velocity or as a duration. The properties involved are `highlightMoveSpeed`, `highlightMoveDuration`, `highlightResizeSpeed` and `highlightResizeDuration`. By default, the speed is set to 400 pixels per second, and the duration is set to -1, indicating that the speed and distance control the duration. If both a speed and a duration is set, the one that results in the quickest animation is chosen.

在默认配置中，视图负责将高亮移动到适当的位置。移动和调整大小的速度可以通过速度或持续时间来控制。涉及的属性是`highlightMoveSpeed`、`highlightMoveDuration`、`highlightResizeSpeed`和`highlightResizeDuration`。默认情况下，速度设置为每秒400个像素，持续时间设置为-1，表示速度和距离控制持续时间。如果同时设置了速度和持续时间，则选择结果最快的动画。

To control the movement of the highlight more in detail, the `highlightFollowCurrentItem` property can be set to `false`. This means that the view is no longer responsible for the movement of the highlight delegate. Instead, the movement can be controlled through a `Behavior` or an animation.

要更详细地控制高亮的移动，可以将`highlightFollowCurrentItem`属性设置为`false`。这意味着视图不再负责高亮代理的移动。相反，可以通过`Behavior`或动画来控制移动。

In the example below, the `y` property of the highlight delegate is bound to the `ListView.view.currentItem.y` attached property. This ensures that the highlight follows the current item. However, as we do not let the view move the highlight, we can control how the element is moved. This is done through the `Behavior on y`. In the example below, the movement is divided into three steps: fading out, moving, before fading in. Notice how `SequentialAnimation` and `PropertyAnimation` elements can be used in combination with the `NumberAnimation` to create a more complex movement.

在下面的示例中，高亮代理的`y`属性绑定到`ListView.view.currentItem.y`附加属性。这确保高亮跟随当前项目。但是，由于我们不允许视图移动高亮，我们可以控制元素如何移动。这是通过`Behavior on y`完成的。在下面的示例中，移动分为三个步骤：淡出、移动，然后淡入。请注意，`SequentialAnimation`和`PropertyAnimation`元素可以与`NumberAnimation`结合使用，以创建更复杂的移动。


<<< @/docs/ch07-modelview/src/listview/highlight-custom.qml#highlight-component

## Header and Footer(头部和尾部)

At each end of the `ListView` contents, a `header` and a `footer` element can be inserted. These can be considered special delegates placed at the beginning or end of the list. For a horizontal list, these will not appear at the head or foot, but rather at the beginning or end, depending on the `layoutDirection` used.

在`ListView`内容的两端，可以插入一个`header`和一个`footer`元素。这些可以被认为是放在列表开头或结尾的特殊代理。对于水平列表，这些不会出现在头部或尾部，而是根据使用的`layoutDirection`出现在开头或结尾。


The example below illustrates how a header and footer can be used to enhance the perception of the beginning and end of a list. There are other uses for these special list elements. For instance, they can be used to keep buttons to load more contents.

下面的示例说明了如何使用头部和尾部来增强列表开头和结尾的感知。这些特殊列表元素还有其他用途。例如，它们可以用来保持加载更多内容的按钮。


<<< @/docs/ch07-modelview/src/listview/header-footer.qml#global

![image](./assets/automatic/listview-header-footer.png)

::: tip
Header and footer delegates do not respect the `spacing` property of a `ListView`, instead they are placed directly adjacent to the next item delegate in the list. This means that any spacing must be a part of the header and footer items.

头部和尾部代理不尊重`ListView`的`spacing`属性，而是直接放置在列表中下一个项目代理的旁边。这意味着任何间距都必须是头部和尾部项目的一部分。
:::

## The GridView

Using a `GridView` is very similar to using a `ListView`. The only real difference is that the grid view places the delegates in a two-dimensional grid instead of in a linear list.

使用`GridView`与使用`ListView`非常相似。唯一的真正区别是网格视图将代理放置在二维网格中，而不是线性列表中。

![image](./assets/automatic/gridview-basic.png)

Compared to a list view, the grid view does not rely on spacing and the size of its delegates. Instead, it uses the `cellWidth` and `cellHeight` properties to control the dimensions of the contents delegates. Each delegate item is then placed in the top left corner of each such cell.

与列表视图相比，网格视图不依赖于间距和其代理的大小。相反，它使用`cellWidth`和`cellHeight`属性来控制内容代理的尺寸。然后，每个代理项被放置在每个这样的单元格的左上角。

<<< @/docs/ch07-modelview/src/gridview/basic.qml#global

A `GridView` contains headers and footers, can use a highlight delegate and supports snap modes as well as various bounds behaviors. It can also be orientated in different directions and orientations.

`GridView`包含头部和尾部，可以使用高亮代理，并支持快照模式以及各种边界行为。它还可以在不同方向和方向上定向。


The orientation is controlled using the `flow` property. It can be set to either `GridView.LeftToRight` or `GridView.TopToBottom`. The former value fills a grid from the left to the right, adding rows from the top to the bottom. The view is scrollable in the vertical direction. The latter value adds items from the top to the bottom, filling the view from left to right. The scrolling direction is horizontal in this case.

方向使用`flow`属性控制。它可以设置为`GridView.LeftToRight`或`GridView.TopToBottom`。前者值从左到右填充网格，从上到下添加行。视图在垂直方向上是可滚动的。后者值从上到下添加项目，从左到右填充视图。在这种情况下，滚动方向是水平的。

In addition to the `flow` property, the `layoutDirection` property can adapt the direction of the grid to left-to-right or right-to-left languages, depending on the value used.

除了`flow`属性外，`layoutDirection`属性还可以根据使用的值将网格方向调整为从左到右或从右到左的语言。
