## Summary(总结)

In this chapter, we have looked at creating QML elements dynamically. This lets us create QML scenes freely, opening the door for user configurable and plug-in based architectures.

在本章中，我们研究了如何动态创建QML元素。这让我们可以自由创建QML场景，为用户可配置和基于插件的体系结构打开了大门

The easiest way to dynamically load a QML element is to use a `Loader` element. This acts as a placeholder for the contents being loaded.

最简单的动态加载QML元素的方法是使用`Loader`元素。这个元素充当正在加载的内容的占位符。


For a more dynamic approach, the `Qt.createQmlObject` function can be used to instantiate a string of QML. This approach does, however, have limitations. The full-blown solution is to dynamically create a `Component` using the `Qt.createComponent` function. Objects are then created by calling the `createObject` function of a `Component`.


对于更动态的方法，Qt.createQmlObject函数可用于实例化QML字符串。然而，这种方法确实有局限性。完整的解决方案是使用Qt.createComponent函数动态创建组件。然后通过调用组件的createObject函数来创建对象。


As bindings and signal connections rely on the existence of an object `id`, or access to the object instantiation, an alternate approach must be used for dynamically created objects. To create a binding, the `Binding` element is used. The `Connections` element makes it possible to connect to signals of a dynamically created object.

由由于绑定和信号连接依赖于对象id的存在或对对象实例化的访问，因此必须为动态创建的对象使用另一种方法。要创建绑定，需要使用Binding元素类型。Connections元素类型可以连接到动态创建的对象的信号。


One of the challenges of working with dynamically created items is to keep track of them. This can be done using a model. By having a model tracking the dynamically created items, it is possible to implement functions for serialization and deserialization, making it possible to store and restore dynamically created scenes.

使用动态创建的项目的一个挑战是跟踪它们。这可以通过使用模型来完成。通过让模型跟踪动态创建的项目，可以实现序列化和反序列化函数，从而可以存储和恢复动态创建的场景。

