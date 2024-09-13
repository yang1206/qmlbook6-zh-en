# Basic Models(基本模型)

The most basic way to visualize data from a model is to use the `Repeater` element. It is used to instantiate an array of items and is easy to combine with a positioner to populate a part of the user interface. A repeater uses a model, which can be anything from the number of items to instantiate, to a full-blown model gathering data from the Internet.

最基础的可视化模型数据的方法是使用`Repeater`元素。Repeater用于实例化一组项目，并且很容易与定位器结合使用来填充用户界面的一部分。Repeater使用一个模型，这个模型可以简单到只是要实例化的项目数量，也可以是一个完整的模型，从互联网收集数据。

## Using a number

In its simplest form, the repeater can be used to instantiate a specified number of items. Each item will have access to an attached property, the variable `index`, that can be used to tell the items apart. 

在最简单的形式中，Repeater可以用来实例化指定数量的项目。每个项目都可以访问一个附加属性，变量`index`，可以用来区分项目。

In the example below, a repeater is used to create 10 instances of an item. The number of items is controlled using the `model` property and their visual representation is controlled using the `delegate` property. For each item in the model, a delegate is instantiated (here, the delegate is a `BlueBox`, which is a customized `Rectangle` containing a `Text` element). As you can tell, the `text` property is set to the `index` value, thus the items are numbered from zero to nine.

在下面的例子中，一个Repeater被用来创建10个项目的实例。项目的数量是通过`model`属性控制的，他们的视觉表示是通过`delegate`属性控制的。对于模型中的每个项目，都会实例化一个代理（这里，代理是一个`BlueBox`，它是一个包含`Text`元素的定制`Rectangle`）。正如你所看到的，`text`属性被设置为`index`值，因此项目从0到9编号。


<<< @/docs/ch07-modelview/src/repeater/number.qml#global

![image](./assets/automatic/repeater-number.png)

## Using an array

As nice as lists of numbered items are, it is sometimes interesting to display a more complex data set. By replacing the integer `model` value with a JavaScript array, we can achieve that. The contents of the array can be of any type, be it strings, integers or objects. In the example below, a list of strings is used. We can still access and use the `index` variable, but we also have access to `modelData` containing the data for each element in the array.

虽然数字列表很有趣，但有时显示更复杂的数据集会更有趣。通过将整数`model`值替换为JavaScript数组，我们可以实现这一点。数组的内容可以是任何类型，可以是字符串、整数或对象。在下面的例子中，使用了一个字符串列表。我们仍然可以访问和使用`index`变量，但我们也可以访问`modelData`，其中包含数组中每个元素的数据。

<<< @/docs/ch07-modelview/src/repeater/array.qml#global

![image](./assets/automatic/repeater-array.png)

## Using a `ListModel`

Being able to expose the data of an array, you soon find yourself in a position where you need multiple pieces of data per item in the array. This is where models enter the picture. One of the most trivial models and one of the most commonly used is the `ListModel`. A list model is simply a collection of `ListElement` items. Inside each list element, a number of properties can be bound to values. For instance, in the example below, a name and a color are provided for each element.

能够暴露数组的数据，你很快就会发现自己处于需要数组中每个项目多个数据的情况。这就是模型进入画面的时候。最简单也是使用最频繁的模型之一是`ListModel`。列表模型只是`ListElement`项目的集合。在每个列表元素中，可以绑定多个属性到值。例如，在下面的例子中，为每个元素提供了名称和颜色。


The properties bound inside each element are attached to each instantiated item by the repeater. This means that the variables `name` and `surfaceColor` are available from within the scope of each `Rectangle` and `Text` item created by the repeater. This not only makes it easy to access the data, it also makes it easy to read the source code. The `surfaceColor` is the color of the circle to the left of the name, not something obscure as data from column `i` of row `j`.

绑定到每个元素的属性由重复器附加到每个实例化的项目上。这意味着`name`和`surfaceColor`变量可以从每个由重复器创建的`Rectangle`和`Text`项目的范围内访问。这不仅使访问数据变得容易，也使阅读源代码变得容易。`surfaceColor`是左边的圆形的颜色，而不是像第`i`列第`j`行的数据那样晦涩难懂。


<<< @/docs/ch07-modelview/src/repeater/model.qml#global

![image](./assets/automatic/repeater-model.png)

## Using a delegate as default property

The `delegate` property of the `Repeater` is its default property. This means that it's also possible to write the code of Example 01 as follows:

`Repeater`的`delegate`属性是其默认属性。这意味着也可以将示例01的代码编写如下：


<<< @/docs/ch07-modelview/src/repeater/delegate.qml#global
