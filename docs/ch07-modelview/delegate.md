# Delegate(代理)

When it comes to using models and views in a custom user interface, the delegate plays a huge role in creating a look and behaviour. As each item in a model is visualized through a delegate, what is actually visible to the user are the delegates.

当涉及到在自定义用户界面中使用模型和视图时，代理在创建外观和行为方面起着巨大的作用。由于模型中的每个项目都是通过代理来可视化的，因此用户实际上看到的就是这些代理。



Each delegate gets access to a number of attached properties, some from the data model, others from the view. From the model, the properties convey the data for each item to the delegate. From the view, the properties convey state information related to the delegate within the view. Let's dive into the properties from the view.

每个代理都可以访问许多附加属性，其中一些来自数据模型，其他来自视图。来自模型，属性将每个项目的数据传达给代理。来自视图，属性传达与视图中的代理相关的状态信息。让我们深入了解来自视图的属性。


The most commonly used properties attached from the view are `ListView.isCurrentItem` and `ListView.view`. The first is a boolean indicating if the item is the current item, while the latter is a read-only reference to the actual view. Through access to the view, it is possible to create general, reusable delegates that adapt to the size and nature of the view in which they are contained. In the example below, the `width` of each delegate is bound to the `width` of the view, while the background `color` of each delegate depends on the attached `ListView.isCurrentItem` property.

最常用的来自视图的属性是 `ListView.isCurrentItem` 和 `ListView.view`。第一个是一个布尔值，指示项目是否是当前项目，而后者是对实际视图的只读引用。通过访问视图，可以创建一般可重用的代理，这些代理适应包含它们的视图的大小和本质。在下面的示例中，每个代理的 `width` 绑定到视图的 `width`，而每个代理的 `background` `color` 都取决于附加的 `ListView.isCurrentItem` 属性。



<<< @/docs/ch07-modelview/src/delegates/basic.qml#global

![image](./assets/automatic/delegates-basic.png)

If each item in the model is associated with an action, for instance, clicking an item acts upon it, that functionality is a part of each delegate. This divides the event management between the view, which handles the navigation between items in the view, and the delegate which handles actions on a specific item.

如果模型中的每个项目都关联一个操作，例如，单击一个项目对其执行操作，那么这个功能是每个代理的一部分。这将在视图和代理之间划分事件管理，视图处理视图中的项目之间的导航，而代理处理特定项目的操作。

The most basic way to do this is to create a `MouseArea` within each delegate and act on the `onClicked` signal. This is demonstrated in the example in the next section of this chapter.

实现这一点最基本的方法是在每个代理内部创建一个MouseArea，并对onClicked信号作出反应。这在本章下一节的例子中有所演示。





## Animating Added and Removed Items（动画添加和删除的项目）

In some cases, the contents shown in a view changes over time. Items are added and removed as the underlying data model is altered. In these cases, it is often a good idea to employ visual cues to give the user a sense of direction and to help the user understand what data is added or removed.

在某些情况下，视图中所显示的内容会随时间而变化。当底层数据模型发生变化时，项目会被添加和删除。在这些情况下，使用视觉提示来给用户一种方向感，并帮助用户了解添加或删除了哪些数据，通常是一个好主意。



Conveniently enough, QML views attach two signals, `onAdd` and `onRemove`, to each item delegate. By triggering animations from  these, it is easy to create the movement necessary to aid the user in identifying what is taking place.

幸运的是，QML视图将两个信号 `onAdd` 和 `onRemove` 附加到每个项目代理中。通过从这些信号触发动画，可以轻松创建必要的运动，以帮助用户识别正在发生的事情。

The example below demonstrates this through the use of a dynamically populated `ListModel`. At the bottom of the screen, a button for adding new items is shown. When it is clicked, a new item is added to the model using the `append` method. This triggers the creation of a new delegate in the view, and the emission of the `GridView.onAdd` signal. The `SequentialAnimation` called `addAnimation` is started from the signal causes the item to zoom into view by animating the `scale` property of the delegate.

下面的示例通过使用动态填充的 `ListModel` 来演示这一点。在屏幕底部，显示了一个用于添加新项目的按钮。当单击它时，使用 `append` 方法将新项目添加到模型中。这会触发在视图中创建一个新代理，并发出 `GridView.onAdd` 信号。从信号中调用的 `SequentialAnimation` 调用 `addAnimation`，通过动画化代理的 `scale` 属性，使项目缩放到视图中。

<<< @/docs/ch07-modelview/src/delegates/add-remove-effects.qml#add-animation

When a delegate in the view is clicked, the item is removed from the model through a call to the `remove` method. This causes the `GridView.onRemove` signal to be emitted, starting the `removeAnimation` `SequentialAnimation`. This time, however, the destruction of the delegate must be delayed until the animation has completed. To do this, `PropertyAction` element is used to set the `GridView.delayRemove` property to `true` before the animation, and `false` after. This ensures that the animation is allowed to complete before the delegate item is removed.

当视图中的代理被点击时，通过调用 `remove` 方法将项目从模型中删除。这会发出 `GridView.onRemove` 信号，启动 `removeAnimation` `SequentialAnimation`。然而，这次必须延迟销毁代理，直到动画完成。为此，在动画之前使用 `PropertyAction` 元素将 `GridView.delayRemove` 属性设置为 `true`，在动画之后设置为 `false`。这确保了在删除代理项目之前允许动画完成。

<<< @/docs/ch07-modelview/src/delegates/add-remove-effects.qml#remove-animation

Here is the complete code:

<<< @/docs/ch07-modelview/src/delegates/add-remove-effects.qml#global

## Shape-Shifting Delegates（形状变化的代理）

A commonly used mechanism in lists is that the current item is expanded when activated. This can be used to dynamically let the item expand to fill the screen to enter a new part of the user interface, or it can be used to provide slightly more information for the current item in a given list.

列表中常用的机制是，当激活时，当前项目会展开。这可以用来动态地让项目展开以填充屏幕，进入用户界面的新部分，或者可以用来为给定列表中的当前项目提供稍多的信息。

In the example below, each item is expanded to the full extent of the `ListView` containing it when clicked. The extra space is then used to add more information. The mechanism used to control this is a state `expanded` that each item delegate can enter, where the item is expanded. In that state, a number of properties are altered.

下面的示例中，当点击每个项目时，该项目会展开到包含它的 `ListView` 的最大范围。然后使用额外的空间来添加更多信息。控制此机制的机制是每个项目代理可以进入的 `expanded` 状态，其中项目被展开。在该状态下，会更改一些属性。

First of all, the `height` of the `wrapper` is set to the height of the `ListView`. The thumbnail image is then enlarged and moved down to make it move from its small position into its larger position. In addition to this, the two hidden items, the `factsView` and `closeButton` are shown by altering the `opacity` of the elements. Finally, the `ListView` is setup.

首先，将 `wrapper` 的高度设置为 `ListView` 的高度。然后，将缩略图图像放大并向下移动，使其从其小位置移动到其大位置。此外，通过更改元素的 `opacity` 来显示两个隐藏的项目，即 `factsView` 和 `closeButton`。最后，设置 `ListView`。


Setting up the `ListView` involves setting the `contentsY`, that is the top of the visible part of the view, to the `y` value of the delegate. The other change is to set `interactive` of the view to `false`. This prevents the view from moving. The user can no longer scroll through the list or change the current item.

设置 `ListView` 包括将 `contentsY`，即视图可见部分的顶部，设置为代理的 `y` 值。另一个更改是将视图的 `interactive` 设置为 `false`。这可以防止视图移动。用户不能再滚动列表或更改当前项目。


As the item first is clicked, it enters the `expanded` state, causing the item delegate to fill the `ListView` and the contents to rearrange. When the close button is clicked, the state is cleared, causing the delegate to return to its previous state and re-enabling the `ListView`.

当项目第一次被点击时，它会进入 `expanded` 状态，导致项目代理填充 `ListView`，内容重新排列。当点击关闭按钮时，清除状态，使代理返回到其先前状态，并重新启用 `ListView`。



<<< @/docs/ch07-modelview/src/delegates/expanding.qml#global

![image](./assets/automatic/delegates-expanding-small.png)

![image](./assets/automatic/delegates-expanding-large.png)

The techniques demonstrated here to expand the delegate to fill the entire view can be employed to make an item delegate shift shape in a much smaller way. For instance, when browsing through a list of songs, the current item could be made slightly larger, accommodating more information about that particular item.

在这里演示的将代理展开以填充整个视图的技术可以用来以更小的方式使项目代理改变形状。例如，当浏览歌曲列表时，当前项目可以稍微放大，容纳有关该特定项目的更多信息。
