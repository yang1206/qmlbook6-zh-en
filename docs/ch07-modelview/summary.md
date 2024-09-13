# Summary(总结)

In this chapter, we have looked at models, views, and delegates. For each data entry in a model, a view instantiates a delegate visualizing the data. This separates the data from the presentation.

在这一章中，我们讨论了模型、视图和委托。对于模型中的每一条数据记录，视图都会实例化一个委托来可视化这些数据。这样就将数据与表示分离开来。



A model can be a single integer, where the `index` variable is provided to the delegate. If a JavaScript array is used as a model, the `modelData` variable represents the data of the current index of the array, while `index` holds the index. For more complex cases, where multiple values need to be provided by each data item, a `ListModel` populated with `ListElement` items is a better solution.

模型可以是一个整数，其中`index`变量会传递给委托。如果使用JavaScript数组作为模型，`modelData`变量表示数组当前索引的数据，而`index`则表示索引。对于更复杂的情况，即每个数据项需要提供多个值，使用`ListElement`填充的`ListModel`是更好的解决方案。

For static models, a `Repeater` can be used as the view. It is easy to combine it with a positioner such as `Row`, `Column`, `Grid` or `Flow` to build user interface parts. For dynamic or large data models, a view such as `ListView`, `GridView`, or `TableView` is more appropriate. These create delegate instances on the fly as they are needed, reducing the number of elements live in the scene at once.

对于静态模型，可以使用`Repeater`作为视图。它很容易与`Row`、`Column`、`Grid`或`Flow`等定位器结合使用，以构建用户界面部分。对于动态或大数据模型，`ListView`、`GridView`或`TableView`等视图更为合适。这些视图在需要时动态创建委托实例，从而减少场景中同时存在的元素数量。

The difference between `GridView` and `TableView` is that the table view expects a table type model with multiple columns of data while the grid view shows a list type model in a grid.

`GridView`和`TableView`之间的区别在于，表格视图期望一个具有多个数据列的表格类型模型，而网格视图以网格形式显示列表类型模型。


The delegates used in the views can be static items with properties bound to data from the model, or they can be dynamic, with states depending on if they are in focus or not. Using the `onAdd` and `onRemove` signals of the view, they can even be animated as they appear and disappear.

视图使用的委托可以是静态项，其属性绑定到模型中的数据，也可以是动态的，其状态取决于它们是否处于焦点。使用视图的`onAdd`和`onRemove`信号，它们甚至可以在出现和消失时进行动画处理。
