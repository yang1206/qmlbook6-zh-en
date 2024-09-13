# Concept(概念)

A common pattern when developing user interfaces is to keep the representation of the data separate from the visualization. This makes it possible to show the same data in different ways depending on what task the user is performing. For instance, a phone book could be arranged as a vertical list of text entries, or as a grid of pictures of the contacts. In both cases, the data is identical: the phone book, but the visualization differs. This division is commonly referred to as the model-view pattern. In this pattern, the data is referred to as the model, while the visualization is handled by the view.

在开发用户界面时，一个常见的模式是将数据的表现形式与其可视化分开。这样做可以根据用户执行的任务以不同的方式展示相同的数据。例如，电话簿可以被排列为文本条目的垂直列表，也可以是联系人照片的网格。在这两种情况下，数据是一样的：即电话簿本身，但是可视化的形式不同。这种划分通常被称为模型-视图模式。在这种模式中，数据被称为模型，而可视化则由视图来处理。


In QML, the model and view are joined by the delegate. The responsibilities are divided as follows: The model provides the data. For each data item, there might be multiple values. In the example above, each phone book entry has a name, a picture, and a number. The data is arranged in a view, in which each item is visualized using a delegate. The task of the view is to arrange the delegates, while each delegate shows the values of each model item to the user.

 在QML中，模型和视图通过代理连接起来。职责划分如下：模型提供数据。对于每一项数据，可能会有多个值。以上述例子来说，每个电话簿条目都有名字、图片和号码。数据在视图中被安排，其中每个项目都通过一个代理来可视化。视图的任务是安排这些代理，而每个代理负责向用户展示每个模型项目的值。

This means that the delegate knows about the contents of the model and how to visualize it. The view knows about the concept of delegates and how to lay them out. The model only knows about the data it is representing.

这意味着代理了解模型的内容以及如何将其可视化。视图了解代理的概念及其布局方式。而模型只了解其代表的数据

![](./assets/model-view-delegate.png)

