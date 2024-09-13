## Loading Components Dynamically(动态加载组件)

The easiest way to dynamically load different parts of QML is to use the `Loader` element. It serves as a placeholder to the item that is being loaded. The item to load is controlled through either the `source` property or the `sourceComponent` property. The former loads the item from a given URL, while the latter instantiates a `Component`.

最简单的动态加载QML部分的方法是使用`Loader`元素。它作为被加载项的占位符。要加载的项是通过`source`属性或`sourceComponent`属性控制的。前者从给定的URL加载项，而后者实例化一个`Component`。

As the loader serves as a placeholder for the item being loaded, its size depends on the size of the item, and vice versa. If the `Loader` element has a size, either by having set `width` and `height` or through anchoring, the loaded item will be given the loader’s size. If the `Loader` has no size, it is resized in accordance to the size of the item being loaded.

由于加载器作为被加载项的占位符，其大小取决于被加载项的大小，反之亦然。如果`Loader`元素有大小，要么通过设置`width`和`height`，要么通过锚定，则加载的项将获得加载器的大小。如果`Loader`没有大小，它将根据被加载项的大小进行调整。

The example described below demonstrates how two separate user interface parts can be loaded into the same space using a `Loader` element. The idea is to have a speed dial that can be either digital or analog, as shown in the illustration below. The code surrounding the dial is unaffected by which item that is loaded for the moment.

下面的示例演示了如何使用`Loader`元素将两个单独的用户界面部分加载到同一空间中。想法是有一个速度表盘，可以是数字的，也可以是模拟的，如下图所示。目前，围绕表盘的代码不受加载的项的影响。


![image](./assets/automatic/loader-analog.png)

![image](./assets/automatic/loader-digital.png)

The first step in the application is to declare a `Loader` element. Notice that the `source` property is left out. This is because the `source` depends on which state the user interface is in.

应用程序的第一步是声明一个`Loader`元素。注意`source`属性被省略了。这是因为`source`取决于用户界面处于什么状态。


```qml
Loader {
    id: dialLoader

    anchors.fill: parent
}
```

In the `states` property of the parent of `dialLoader` a set of `PropertyChanges` elements drives the loading of different QML files depending on the `state`. The `source` property happens to be a relative file path in this example, but it can just as well be a full URL, fetching the item over the web.

在`dialLoader`的父级`states`属性中，一组`PropertyChanges`元素根据`state`驱动加载不同的QML文件。在这个例子中，`source`恰好是一个相对文件路径，但它也可以是一个完整的URL，通过网络获取该项。


<<< @/docs/ch15-dynamicqml/src/loader/main.qml#M3

In order to make the loaded item come alive, it is `speed` property must be bound to the root `speed` property. This cannot be done as a direct binding as the item not always is loaded and changes over time. Instead, a `Binding` element must be used. The `target` property of the binding is changed every time the `Loader` triggers the `onLoaded` signal.

为了使加载的项变得生动起来，`speed`属性必须绑定到根`speed`属性。不能直接绑定，因为项并不总是被加载，并且会随时间变化。相反，必须使用`Binding`元素。每当`Loader`触发`onLoaded`信号时，绑定`target`属性都会发生变化。

<<< @/docs/ch15-dynamicqml/src/loader/main.qml#M1

The `onLoaded` signal lets the loading QML act when the item has been loaded. In a similar fashion, the QML being loaded can rely on the `Component.onCompleted` signal. This signal is actually available for all components, regardless of how they are loaded. For instance, the root component of an entire application can use it to kick-start itself when the entire user interface has been loaded.

`onLoaded`信号让加载的QML在项被加载时发挥作用。以类似的方式，被加载的QML可以依赖`Component.onCompleted`信号。实际上，对于所有组件，无论它们是如何加载的，这个信号都是可用的。例如，整个应用程序的根组件可以使用它来启动自己，当整个用户界面被加载时。

### Connecting Indirectly（间接连接）

When creating QML elements dynamically, you cannot connect to signals using the `onSignalName` approach used for static setup. Instead, the `Connections` element must be used. It connects to any number of signals of a `target` element.

在动态创建QML元素时，不能使用`onSignalName`方法来连接信号，用于静态设置。相反，必须使用`Connections`元素。它连接到`target`元素的任意数量的信号。

Having set the `target` property of a `Connections` element, the signals can be connected, as usual, that is, using the `onSignalName` approach. However, by altering the `target` property, different elements can be monitored at different times.

设置`Connections`元素的`target`属性后，可以像通常那样连接信号，即使用`onSignalName`方法。然而，通过改变`target`属性，可以在不同时间监控不同的元素。

![image](./assets/automatic/connections.png)

In the example shown above, a user interface consisting of two clickable areas is presented to the user. When either area is clicked, it is flashed using an animation. The left area is shown in the code snippet below. In the `MouseArea`, the `leftClickedAnimation` is triggered, causing the area to flash.

在上面的示例中，向用户呈现了一个由两个可点击区域组成的用户界面。当点击任何一个区域时，它都会使用动画闪烁。下面的代码片段显示了左区域。在`MouseArea`中，触发了`leftClickedAnimation`，使区域闪烁。


<<< @/docs/ch15-dynamicqml/src/connections/main.qml#M1

In addition to the two clickable areas, a `Connections` element is used. This triggers the third animation when the active, i.e. the `target` of the element, is clicked.

除了两个可点击区域外，还使用了一个`Connections`元素。当点击活动元素时，即该元素的`target`，会触发第三个动画。

<<< @/docs/ch15-dynamicqml/src/connections/main.qml#M2

To determine which `MouseArea` to target, two states are defined. Notice that we cannot set the `target` property using a `PropertyChanges` element, as it already contains a `target` property. Instead a `StateChangeScript` is utilized.

为了确定要定位哪个`MouseArea`，定义了两个状态。注意，我们不能使用`PropertyChanges`元素设置`target`属性，因为它已经包含了一个`target`属性。相反，使用`StateChangeScript`。

<<< @/docs/ch15-dynamicqml/src/connections/main.qml#M3

When trying out the example, it is worth noticing that when multiple signal handlers are used, all are invoked. The execution order of these is, however, undefined.

当尝试使用示例时，值得注意的是，当使用多个信号处理程序时，都会被调用。然而，这些的执行顺序是未定义的。


When creating a `Connections` element without setting the `target` property, the property defaults to `parent`. This means that it has to be explicitly set to `null` to avoid catching signals from the `parent` until the `target` is set. This behavior does make it possible to create custom signal handler components based on a `Connections` element. This way, the code reacting to the signals can be encapsulated and re-used.

创建`Connections`元素而不设置`target`属性时，该属性默认为`parent`。这意味着必须将其显式设置为`null`，以避免在设置`target`之前捕获来自`parent`的信号。这种行为确实可以基于`Connections`元素创建自定义信号处理程序组件。这样，对信号的响应代码可以封装并重用。


In the example below, the `Flasher` component can be put inside any `MouseArea`. When clicked, it triggers an animation, causing the parent to flash. In the same `MouseArea` the actual task being triggered can also be carried out. This separates the standardized user feedback, i.e. the flashing, from the actual action.

在下面的示例中，可以将`Flasher`组件放入任何`MouseArea`中。当点击时，它会触发一个动画，使父级闪烁。在同一个`MouseArea`中，还可以执行实际触发的任务。这使标准化的用户反馈，即闪烁，与实际操作分离。

<<< @/docs/ch15-dynamicqml/src/connections-parent/Flasher.qml#M1

To use the `Flasher`, simply instantiate a Flasher within each MouseArea, and it all works.

要使用`Flasher`，只需在每个MouseArea中实例化一个Flasher，它就会工作。


<<< @/docs/ch15-dynamicqml/src/connections-parent/main.qml#M1

When using a `Connections` element to monitor the signals of multiple types of `target` elements, you sometimes find yourself in a situation where the available signals vary between the targets. This results in the `Connections` element outputting run-time errors as signals are missed. To avoid this, the `ignoreUnknownSignal` property can be set to `true`. This ignores all such errors.

当使用`Connections`元素监视多种`target`元素的信号时，有时会遇到可用信号在目标之间不同的情况。这导致`Connections`元素输出运行时错误，因为错过了信号。为了避免这种情况，可以将`ignoreUnknownSignal`属性设置为`true`。这将忽略所有此类错误。

::: tip
It is usually a bad idea to suppress error messages, and if you do, make sure to document why in a comment.

通常抑制错误消息不是一个好主意，如果你这样做，确保在注释中记录为什么。
:::

### Binding Indirectly（间接绑定）

Just as it is not possible to connect to signals of dynamically created elements directly, nor it is possible to bind properties of a dynamically created element without working with a bridge element. To bind a property of any element, including dynamically created elements, the `Binding` element is used.

与直接连接到动态创建元素的信号或使用桥接元素绑定动态创建元素的属性不同，无法使用`Binding`元素绑定任何元素的属性，包括动态创建的元素。要绑定任何元素的属性，包括动态创建的元素，可以使用`Binding`元素。

The `Binding` element lets you specify a `target` element, a `property` to bind and a `value` to bind it to. Through using a `Binding` element, it is, for instance, possible to bind properties of a dynamically loaded element. This was demonstrated in the introductory example in this chapter, as shown below.

`Binding`元素允许您指定`target`元素、要绑定的属性以及要绑定到的值。通过使用`Binding`元素，可以绑定动态加载元素的属性。例如，在本章的介绍示例中进行了演示，如下所示。

<<< @/docs/ch15-dynamicqml/src/loader/main.qml#M1

As the `target` element of a `Binding` not always is set, and perhaps not always has a given property, the `when` property of the `Binding` element can be used to limit the time when the binding is active. For instance, it can be limited to specific modes in the user interface.

由于`Binding`的`target`元素不一定总是设置，也不一定具有给定的属性，因此可以使用`Binding`元素的`when`属性来限制绑定活动的时间。例如，可以限制用户界面中的特定模式。

The `Binding` element also comes with a `delayed` property. When this property is set to `true` the binding is not propagated to the `target` until the event queue has been emptied. In high load situations this can serve as an optimization as intermediate values are not pushed to the `target`.

`Binding`元素还附带一个`delayed`属性。当此属性设置为`true`时，绑定不会传播到`target`，直到事件队列被清空。在高负载情况下，这可以作为一种优化，因为中间值不会推送到`target`。

