## Tracking Dynamic Objects（跟踪动态对象）

Working with dynamic objects, it is often necessary to track the created objects. Another common feature is to be able to store and restore the state of the dynamic objects. Both these tasks are easily handled using an `XmlListModel` that is dynamically populated.

使用动态对象时，通常需要跟踪创建的对象。另一个常见功能是能够存储和恢复动态对象的状态。这两项任务都可以使用动态填充的`XmlListModel`轻松处理。

In the example shown below two types of elements, rockets and UFOs can be created and moved around by the user. In order to be able to manipulate the entire scene of dynamically created elements, we use a model to track the items.

在下面的示例中，用户可以创建和移动两种类型的元素，即火箭和UFO。为了能够操作整个动态创建元素的场景，我们使用一个模型来跟踪这些元素。


The model, a `XmlListModel`, is populated as the items are created. The object reference is tracked alongside the source URL used when instantiating it. The latter is not strictly needed for tracking the objects but will come in handy later.

模型，即`XmlListModel`，在创建项目时填充。对象引用与实例化时使用的源URL一起跟踪。后者在跟踪对象方面并不严格需要，但以后会派上用场。


<<< @/docs/ch15-dynamicqml/src/dynamic-scene/main.qml#M1

As you can tell from the example above, the `create-object.js` is a more generalized form of the JavaScript introduced earlier. The `create` method uses three arguments: a source URL, a root element, and a callback to invoke when finished. The callback gets called with two arguments: a reference to the newly created object and the source URL used.

如上例所示，`create-object.js`是前面介绍的JavaScript的更通用的形式。`create`方法使用三个参数：一个源URL、一个根元素和一个回调，在完成时调用。回调使用两个参数调用：新创建对象的引用和使用的源URL。

This means that each time `addUfo` or `addRocket` functions are called, the `itemAdded` function will be called when the new object has been created. The latter will append the object reference and source URL to the `objectsModel` model.

这意味着每次调用`addUfo`或`addRocket`函数时，当新对象被创建时，`itemAdded`函数将被调用。后者会将对象引用和源URL附加到`objectsModel`模型中。

The `objectsModel` can be used in many ways. In the example in question, the `clearItems` function relies on it. This function demonstrates two things. First, how to iterate over the model and perform a task, i.e. calling the `destroy` function for each item to remove it. Secondly, it highlights the fact that the model is not updated as objects are destroyed. Instead of removing the model item connected to the object in question, the `obj` property of that model item is set to `null`. To remedy this, the code explicitly has to clear the model item as the objects are removed.

`objectsModel`可以以多种方式使用。在所讨论的示例中，`clearItems`函数依赖于它。这个函数展示了两个事情。首先，如何遍历模型并执行任务，即对每个项目调用`destroy`函数以删除它。其次，它强调了模型在对象被销毁时并没有更新。相反，不是删除与问题对象关联的模型项，而是将该模型项的`obj`属性设置为`null`。为了弥补这一点，代码必须明确地清除模型项，当对象被删除时。

<<< @/docs/ch15-dynamicqml/src/dynamic-scene/main.qml#M2

Having a model representing all dynamically created items, it is easy to create a function that serializes the items. In the example code, the serialized information consists of the source URL of each object along its `x` and `y` properties. These are the properties that can be altered by the user. The information is used to build an XML document string.

拥有一个表示所有动态创建项目的模型，很容易创建一个序列化项目的函数。在示例代码中，序列化的信息包括每个对象的源URL及其`x`和`y`属性。这些是用户可以更改的属性。这些信息用于构建一个XML文档字符串。

<<< @/docs/ch15-dynamicqml/src/dynamic-scene/main.qml#M3

::: tip
Currently, the `XmlListModel` of Qt 6 lacks the `xml` property and `get()` function needed to make serialization and deserialization work.

当前，Qt 6的`XmlListModel`缺少使序列化和反序列化工作所需的`xml`属性和`get()`函数。
:::

The XML document string can be used with an `XmlListModel` by setting the `xml` property of the model. In the code below, the model is shown along the `deserialize` function. The `deserialize` function kickstarts the deserialization by setting the `dsIndex` to refer to the first item of the model and then invoking the creation of that item. The callback, `dsItemAdded` then sets that `x` and `y` properties of the newly created object. It then updates the index and creates the next object, if any.

通过设置模型的`xml`属性，可以使用`XmlListModel`与XML文档字符串一起使用。在下面的代码中，模型与`deserialize`函数一起显示。`deserialize`函数通过将`dsIndex`设置为引用模型的第一个项目，然后调用该项目的创建来启动反序列化。回调`dsItemAdded`然后设置新创建对象的`x`和`y`属性。然后更新索引并创建下一个对象（如果有）。

<<< @/docs/ch15-dynamicqml/src/dynamic-scene/main.qml#M4

The example demonstrates how a model can be used to track created items, and how easy it is to serialize and deserialize such information. This can be used to store a dynamically populated scene such as a set of widgets. In the example, a model was used to track each item.

示例演示了如何使用模型来跟踪创建的项目，以及如何轻松地序列化和反序列化此类信息。这可以用来存储动态填充的场景，如一组小部件。在示例中，使用模型来跟踪每个项目。

An alternate solution would be to use the `children` property of the root of a scene to track items. This, however, requires the items themselves to know the source URL to use to re-create them. It also requires us to implement a way to be able to tell dynamically created items apart from the items that are a part of the original scene, so that we can avoid attempting to serialize and later deserialize any of the original items.

另一种解决方案是使用场景根部的`children`属性来跟踪项目。然而，这要求项目本身知道要使用的源URL来重新创建它们。它还要求我们实现一种方法，能够区分动态创建的项目和原始场景的一部分，这样我们就可以避免尝试序列化和反序列化任何原始项目。
