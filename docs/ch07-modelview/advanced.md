# Advanced Techniques（高级技术）

## The PathView

The `PathView` element is the most flexible view provided in Qt Quick, but it is also the most complex. It makes it possible to create a view where the items are laid out along an arbitrary path. Along the same path, attributes such as scale, opacity and more can be controlled in detail.

`PathView`元素是Qt Quick提供的最灵活的视图，但同时也是最复杂的。它使得创建一个项目沿着任意路径布局的视图成为可能。沿着相同的路径，可以详细地控制诸如缩放、透明度等属性。




When using the `PathView`, you have to define a delegate and a path. In addition to this, the `PathView` itself can be customized through a range of properties. The most common being `pathItemCount`, controlling the number of visible items at once, and the highlight range control properties `preferredHighlightBegin`, `preferredHighlightEnd` and `highlightRangeMode`, controlling where along the path the current item is to be shown.

使用`PathView`时，必须定义一个代理和一个路径。此外，`PathView`本身可以通过一系列属性进行自定义。最常见的是`pathItemCount`，控制同时可见的项目数量，以及高亮范围控制属性`preferredHighlightBegin`、`preferredHighlightEnd`和`highlightRangeMode`，控制路径上当前项目显示的位置。

Before looking at the highlight range control properties in depth, we must look at the `path` property. The `path` property expects a `Path` element defining the path that the delegates follow as the `PathView` is being scrolled. The path is defined using the `startX` and `startY` properties in combinations with path elements such as `PathLine`, `PathQuad` and `PathCubic`.  These elements are joined together to form a two-dimensional path.

在深入探讨高亮范围控制属性之前，我们必须看一下`path`属性。`path`属性期望一个`Path`元素，该元素定义了随着`PathView`滚动而跟随的代理的路径。路径使用`startX`和`startY`属性以及`PathLine`、`PathQuad`和`PathCubic`等路径元素组合起来定义。这些元素连接在一起形成一个二维路径。



When the path has been defined, it is possible to further tune it using `PathPercent` and `PathAttribute` elements. These are placed in between path elements and provide more fine-grained control over the path and the delegates on it. The `PathPercent` controls how large a portion of the path that has been covered between each element. This, in turn, controls the distribution of delegates along the path, as they are distributed proportionally to the percentage progressed.

定义路径后，可以使用`PathPercent`和`PathAttribute`元素进一步调整路径。这些元素放置在路径元素之间，并提供对路径及其上代理的更精细的控制。`PathPercent`控制每个元素之间路径覆盖的部分有多大。反过来，这控制了沿着路径的代理的分布，因为它们是按比例分布的。

This is where the `preferredHighlightBegin` and `preferredHighlightEnd` properties of the `PathView` enters the picture. They both expect real values in the range between zero and one. The end is also expected to be more or equal to the beginning. Setting both these properties too, for instance, 0.5, the current item will be displayed at the location fifty percent along the path.

这就是`PathView`的`preferredHighlightBegin`和`preferredHighlightEnd`属性发挥作用的地方。它们都期望在零和一之间的实数。结束值也期望大于或等于开始值。例如，将这两个属性都设置为0.5，当前项目将在路径的50%位置显示。



In the `Path`, the `PathAttribute` elements are placed between elements, just as `PathPercent` elements. They let you specify property values that are interpolated along the path. These properties are attached to the delegates and can be used to control any conceivable property.

在`Path`中，`PathAttribute`元素放置在元素之间，就像`PathPercent`元素一样。它们允许您指定沿路径插值的属性值。这些属性附加到代理上，可以用来控制任何可想象的属性。

![image](./assets/automatic/pathview-coverview.png)

The example below demonstrates how the `PathView` element is used to create a view of cards that the user can flip through. It employs a number of tricks to do this. The path consists of three `PathLine` elements. Using `PathPercent` elements, the central element is properly centered and provided enough space not to be cluttered by other elements. Using `PathAttribute` elements, the rotation, size and `z`-value is controlled.

下面的示例演示了如何使用`PathView`元素创建用户可以浏览的卡片视图。它使用了许多技巧来实现这一点。路径由三个`PathLine`元素组成。使用`PathPercent`元素，中央元素被正确居中，并提供了足够的空间，不会被其他元素混乱。使用`PathAttribute`元素，控制旋转、大小和`z`值。

In addition to the `path`, the `pathItemCount` property of the `PathView` has been set. This controls how densely populated the path will be. The `preferredHighlightBegin` and `preferredHighlightEnd` the `PathView.onPath` is used to control the visibility of the delegates.

除了`path`之外，`PathView`的`pathItemCount`属性已经被设置。这控制了路径的密度。`preferredHighlightBegin`和`preferredHighlightEnd`的`PathView.onPath`用于控制代理的可见性。

<<< @/docs/ch07-modelview/src/pathview/coverview.qml#pathview

The delegate, shown below, utilizes the attached properties `itemZ`, `itemAngle` and `itemScale` from the `PathAttribute` elements. It is worth noticing that the attached properties of the delegate only are available from the `wrapper`. Thus, the `rotX` property is defined to be able to access the value from within the `Rotation` element.

代理，如下所示，利用了来自`PathAttribute`元素的附加属性`itemZ`、`itemAngle`和`itemScale`。值得注意的是，代理的附加属性仅在`wrapper`中可用。因此，定义了`rotX`属性，以便从`Rotation`元素中访问该值。


Another detail specific to `PathView` worth noticing is the usage of the attached `PathView.onPath` property. It is common practice to bind the visibility to this, as this allows the `PathView` to keep invisible elements for caching purposes. This can usually not be handled through clipping, as the item delegates of a `PathView` are placed more freely than the item delegates of `ListView` or `GridView` views.

值得注意的`PathView`的另一个细节是使用附加的`PathView.onPath`属性。通常的做法是将可见性绑定到这个属性上，因为这样可以让`PathView`为了缓存目的而保持不可见的元素。通常无法通过剪裁来处理这个问题，因为`PathView`的项目代理比`ListView`或`GridView`视图的项目代理放置得更自由。

<<< @/docs/ch07-modelview/src/pathview/coverview.qml#delegate

When transforming images or other complex elements on in `PathView`, a performance optimization trick that is common to use is to bind the `smooth` property of the `Image` element to the attached property `PathView.view.moving`. This means that the images are less pretty while moving but smoothly transformed when stationary. There is no point spending processing power on smooth scaling when the view is in motion, as the user will not be able to see this anyway.

在`PathView`中转换图像或其他复杂元素时，使用性能优化技巧是常见的做法，将`Image`元素的`smooth`属性绑定到附加属性`PathView.view.moving`。这意味着在静止时图像平滑转换，但在移动时不太美观。当视图在运动中时，没有必要花费处理能力来进行平滑缩放，因为用户无论如何都看不到这一点。


::: tip
Given the dynamic nature of `PathAttribute`, the qml tooling (in this case: `qmllint`) is not aware of `itemZ`, `itemAngle` nor `itemScale`.

鉴于`PathAttribute`的动态特性，qml工具（在本例中：`qmllint`）不知道`itemZ`、`itemAngle`和`itemScale`。
:::

When using the `PathView` and changing the `currentIndex` programmatically you might want to control the direction that the path moves in. You can do this using the `movementDirection` property. It can be set to `PathView.Shortest`, which is the default value. This means that the movement can be either direction, depending on which way is the closest way to move to the target value. The direction can instead be restricted by setting `movementDirection` to `PathView.Negative` or `PathView.Positive`.

当使用`PathView`并编程更改`currentIndex`时，您可能希望控制路径移动的方向。您可以使用`movementDirection`属性来实现。它可以设置为`PathView.Shortest`，这是默认值。这意味着移动可以是任一方向，取决于移动到目标值的最短距离。相反，可以通过将`movementDirection`设置为`PathView.Negative`或`PathView.Positive`来限制方向。

## Table Models

All views discussed until now present an array of items one way or another. Even the `GridView` expects the model to provide a one dimensional list of items. For two dimensional tables of data you need to use the `TableView` element.

到目前为止讨论的所有视图都以某种方式呈现一组项目。即使是`GridView`也期望模型提供一个一维的项目列表。对于数据的二维表格，您需要使用`TableView`元素。

The `TableView` is similar to other views in that it combines a `model` with a `delegate` to form a grid. If given a list oriented model, it displays a single column, making it very similar to the `ListView` element. However, it can also display two-dimensional models that explicitly define both columns and rows.

`TableView`与其他视图类似，它将`model`与`delegate`组合起来形成一个网格。如果给定一个列表模型，它将显示一列，使其非常类似于`ListView`元素。然而，它也可以显示显式定义列和行的二维模型。

In the example below, we set up a simple `TableView` with a custom model exposed from C++. At the moment, it is not possible to create table oriented models directly from QML, but in the ‘Qt and C++’ chapter the concept is explained. The running example is shown in the image below.

在下面的示例中，我们设置了一个简单的`TableView`，并从C++中暴露了一个自定义模型。目前，无法直接从QML创建面向表的模型，但在“Qt和C++”章节中解释了该概念。运行中的示例如下图所示。

![image](./assets/tableview.png)

In the example below, we create a `TableView` and set the `rowSpacing` and `columnSpacing` to control the horizontal and vertical gaps between delegates. The rest of the properties are set up as for any other type of view.

在下面的示例中，我们创建了一个`TableView`，并将`rowSpacing`和`columnSpacing`设置为控制代理之间的水平和垂直间隙。其余的属性设置与其他类型的视图相同。

<<< @/docs/ch07-modelview/src/tableview/main.qml#tableview

The delegate itself can carry an implicit size through the `implicitWidth` and `implicitHeight`. This is what we do in the example below. The actual data contents, i.e. the data returned from the model’s `display` role.

代理本身可以通过`implicitWidth`和`implicitHeight`携带隐式大小。这就是我们在下面的示例中所做的。实际的数据内容，即模型`display`角色返回的数据。


<<< @/docs/ch07-modelview/src/tableview/main.qml#delegate

It is possible to provide delegates with different sizes depending on the model contents, e.g.:

可以根据模型内容的不同，为委代理提供不同的大小，例如:

```qml
GreenBox {
    implicitHeight: (1 + row) * 10
    // ...
}
```

Notice that both the width and the height must be greater than zero.

注意，宽度和高度都必须大于零。


When providing an implicit size from the delegate, the tallest delegate of each row and the widest delegate of each column controls the size. This can create interesting behaviour if the width of items depend on the row, or if the height depends on the column. This is because not all delegates are instantiated at all times, so the width of a column might change as the user scrolls through the table.

当从代理提供隐式大小时，每一行中最高的代理和每一列中最宽的代理控制大小。如果项目的宽度取决于行，或者高度取决于列，这可能会创建有趣的行为。这是因为并非所有代理都始终被实例化，因此随着用户滚动浏览表格，列的宽度可能会发生变化。



To avoid the issues with specifying column widths and row heights using implicit delegate sizes, you can provide functions that calculate these sizes. This is done using the `columnWidthProvider` and `rowHeightProvider` . These functions return the size of the width and row respectively as shown below:

为了避免使用隐式代理大小指定列宽和行高时出现的问题，您可以使用函数来计算这些大小。这是使用`columnWidthProvider`和`rowHeightProvider`完成的。这些函数分别返回宽度和行的大小，如下所示:


```qml
TableView {
    columnWidthProvider: function (column) { return 10 * (column + 1) }
    // ...
}
```

If you need to dynamically change the column widths or row heights you must notify the view of this by calling the `forceLayout` method. This will make the view re-calculate the size and position of all cells.

如果您需要动态更改列宽或行高，您必须通过调用`forceLayout`方法通知视图。这将使视图重新计算所有单元格的大小和位置。

## A Model from XML

As XML is a ubiquitous data format, QML provides the `XmlListModel` element that exposes XML data as a model. The element can fetch XML data locally or remotely and then processes the data using XPath expressions.

由于XML是一种普遍的数据格式，QML提供了`XmlListModel`元素，该元素将XML数据作为模型公开。该元素可以本地或远程获取XML数据，然后使用XPath表达式处理数据。

The example below demonstrates fetching images from an RSS flow. The `source` property refers to a remote location over HTTP, and the data is automatically downloaded.

下面的示例演示了从RSS流中获取图像。`source`属性指的是通过HTTP的远程位置，数据会自动下载。

![image](./assets/automatic/xmllistmodel-images.png)

When the data has been downloaded, it is processed into model items and roles. The `query` property of the `XmlListModel` is an XPath representing the base query for creating model items. In this example, the path is `/rss/channel/item`, so for every item tag, inside a channel tag, inside an RSS tag, a model item is created.

数据下载后，它会被处理成模型项和角色。`XmlListModel`的`query`属性是一个表示创建模型项的基本查询的XPath。在这个例子中，路径是`/rss/channel/item`，因此对于`rss`标签内的`channel`标签内的每个`item`标签，都会创建一个模型项。

For every model item, a number of roles are extracted. These are represented by `XmlListModelRole` elements. Each role is given a name, which the delegate can access through an attached property. The actual value of each such property is determined through the `elementName` and (optional) `attributeName` properties for each role. For instance, the `title` property corresponds to the `title` XML element, returning the contents between the `<title>` and `</title>` tags.

对于每个模型项，都会提取多个角色。这些由`XmlListModelRole`元素表示。每个角色都被赋予一个名称，代理可以通过附加属性访问该名称。每个此类属性的值由每个角色的`elementName`属性和（可选）`attributeName`属性确定。例如，`title`属性对应于`title` XML元素，返回`<title>`和`</title>`标签之间的内容。

The `imageSource` property extracts the value of an attribute of a tag instead of the contents of the tag. In this case, the `url` attribute of the `enclosure` tag is extracted as a string. The `imageSource` property can then be used directly as the `source` for an `Image` element, which loads the image from the given URL.

`imageSource`属性提取标签的属性值，而不是标签的内容。在这种情况下，提取`enclosure`标签的`url`属性的值作为一个字符串。然后，`imageSource`属性可以直接用作`Image`元素的`source`，从给定的URL加载图像。

<<< @/docs/ch07-modelview/src/xmllistmodel/images.qml#global

## Lists with Sections()

Sometimes, the data in a list can be divided into sections. It can be as simple as dividing a list of contacts into sections under each letter of the alphabet or music tracks under albums. Using a `ListView` it is possible to divide a flat list into categories, providing more depth to the experience.

有时，列表中的数据可以分成多个部分。它可以像将联系人列表分成按字母顺序排列的各个部分一样简单，或者像将音乐曲目分成专辑一样简单。使用`ListView`，可以将一个平面列表分成多个类别，为体验提供更多的深度。

![image](./assets/automatic/listview-sections.png)

In order to use sections, the `section.property` and `section.criteria` must be set up. The `section.property` defines which property to use to divide the contents into sections. Here, it is important to know that the model must be sorted so that each section consists of continuous elements, otherwise, the same property name might appear in multiple locations.

为了使用部分，必须设置`section.property`和`section.criteria`。`section.property`定义了将内容分成部分时要使用的属性。在这里，重要的是要知道模型必须被排序，以便每个部分由连续的元素组成，否则，相同的属性名称可能会出现在多个位置。

The `section.criteria` can be set to either `ViewSection.FullString` or `ViewSection.FirstCharacter`. The first is the default value and can be used for models that have clear sections, for example, tracks of music albums. The latter takes the first character of a property and means that any property can be used for this. The most common example being the last name of contacts in a phone book.

`section.criteria`可以设置为`ViewSection.FullString`或`ViewSection.FirstCharacter`。第一个是默认值，可以用于有明确部分的模型，例如音乐专辑的曲目。后者取属性的第一个字符，意味着任何属性都可以使用。最常见的例子是电话簿中联系人的姓氏。

When the sections have been defined, they can be accessed from each item using the attached properties `ListView.section`, `ListView.previousSection` and `ListView.nextSection`. Using these properties, it is possible to detect the first and last item of a section and act accordingly.

定义部分后，可以使用附加属性`ListView.section`、`ListView.previousSection`和`ListView.nextSection`从每个项中访问它们。使用这些属性，可以检测到每个部分的第一个和最后一个项，并相应地操作。


It is also possible to assign a section delegate component to the `section.delegate` property of a `ListView`. This creates a section header delegate which is inserted before any items of a section. The delegate component can access the name of the current section using the attached property `section`.

还可以将部分委托组件分配给`ListView`的`section.delegate`属性。这将在任何部分的项之前插入一个部分标题委托。委托组件可以使用附加属性`section`访问当前部分的名称。

The example below demonstrates the section concept by showing a list of spacemen sectioned after their nationality. The `nation` is used as the `section.property`. The `section.delegate` component, `sectionDelegate`, shows a heading for each nation, displaying the name of the nation. In each section, the names of the spacemen are shown using the `spaceManDelegate` component.

下面的示例通过根据宇航员的国家将宇航员列表分成多个部分来演示部分的概念。`nation`被用作`section.property`。`section.delegate`组件`sectionDelegate`为每个国家显示一个标题，显示国家的名称。在每个部分中，使用`spaceManDelegate`组件显示宇航员的姓名。


<<< @/docs/ch07-modelview/src/listview/sections.qml#global

## The ObjectModel

In some cases you might want to use a list view for a large set of different items. You can solve this using dynamic QML and `Loader`, but another options is to use an `ObjectModel` from the `QtQml.Models` module. The object model is different from other models as it lets you put the actual visual elements side the model. That way, the view does not need any `delegate`.


在某些情况下，您可能希望使用一个列表视图来显示大量不同的项目。您可以使用动态QML和`Loader`来解决这个问题，但另一种选择是使用`QtQml.Models`模块中的`ObjectModel`。对象模型与其他模型不同，因为它允许您将实际的视觉元素放在模型中。这样，视图就不需要任何`delegate`。


![image](./assets/automatic/delegates-objectmodel.png)

In the example below we put three `Rectangle` elements into the `ObjectModel`. However, one rectangle has a `Text` element child while the last one has rounded corners. This would have resulted in a table-style model using something like a `ListModel`. It would also have resulted in empty `Text` elements in the model.

下面的示例将三个`Rectangle`元素放入`ObjectModel`中。但是，一个矩形有一个`Text`元素子元素，而最后一个矩形有圆角。这会导致使用类似`ListModel`的表格式模型。这也意味着模型中会有空的`Text`元素。

<<< @/docs/ch07-modelview/src/delegates/objectmodel.qml#global

Another aspect of the `ObjectModel` is that is can be dynamically populated using the `get`, `insert`, `move`, `remove`, and `clear` methods. This way, the contents of the model can be dynamically generated from various sources and still easily shown in a single view.

`ObjectModel`的另一个方面是它可以使用`get`、`insert`、`move`、`remove`和`clear`方法动态填充。这样，模型的内容可以从各种来源动态生成，并轻松地在单个视图中显示。

## Models with Actions

The `ListElement` type supports the binding of Javascript functions to properties. This means that you can put functions into a model. This is very useful when building menus with actions and similar constructs.

`ListElement`类型支持将Javascript函数绑定到属性。这意味着您可以将函数放入模型中。当构建带有操作和类似结构的菜单时，这非常有用。

The example below demonstrates this by having a model of cities that greet you in different ways. The `actionModel` is a model of four cities, but the `hello` property is bound to functions. Each function takes an argument `value`, but you can have any number arguments.

下面的示例通过拥有一个以不同方式向您打招呼的城市模型来演示这一点。`actionModel`是一个由四个城市组成的模型，但`hello`属性绑定到函数。每个函数接受一个参数`value`，但您可以有任意数量的参数。


In the delegate `actionDelegate`, the `MouseArea` calls the function `hello` as an ordinary function and this results a call to the corresponding `hello` property in the model.

在委托`actionDelegate`中，`MouseArea`将函数`hello`作为普通函数调用，这导致对模型中相应的`hello`属性进行调用。

<<< @/docs/ch07-modelview/src/delegates/model-action.qml#global

## Tuning Performance

The perceived performance of a view of a model depends very much on the time needed to prepare new delegates. For instance, when scrolling downwards through a ListView, delegates are added just outside the view from the bottom and are removed just as they leave sight over the top of the view. This becomes apparent if the `clip` property is set to `false`. If the delegates take too much time to initialize, it will become apparent to the user as soon as the view is scrolled too quickly.


模型视图的性能感知取决于准备新委托所需的时间。例如，当向下滚动ListView时，委托会从视图底部外部添加，并在它们离开视图顶部时被移除。如果将`clip`属性设置为`false`，则变得明显。如果委托初始化时间过长，一旦视图滚动得太快，用户就会明显感觉到。

To work around this issue you can tune the margins, in pixels, on the sides of a scrolling view. This is done using the `cacheBuffer` property. In the case described above, vertical scrolling, it will control how many pixels above and below the ListView that will contain prepared delegates. Combining this with asynchronously loading `Image` elements can, for instance, give the images time to load before they are brought into view.

要解决此问题，您可以在滚动视图的两侧调整边距（以像素为单位）。这是使用`cacheBuffer`属性完成的。在上面的情况下，垂直滚动，它将控制ListView上方和下方将包含准备好的委托的像素数。例如，结合异步加载`Image`元素，可以在将它们带入视图之前给图像加载时间。

Having more delegates sacrifices memory for a smoother experience and slightly more time to initialize each delegate. This does not solve the problem of complex delegates. Each time a delegate is instantiated, its contents are evaluated and compiled. This takes time, and if it takes too much time, it will lead to a poor scrolling experience. Having many elements in a delegate will also degrade the scrolling performance. It simply costs cycles to move many elements.

拥有更多的委托会牺牲内存以获得更流畅的体验，并且每个委托的初始化时间会稍微长一些。这并不能解决复杂委托的问题。每次实例化委托时，都会评估其内容并编译。这需要时间，如果时间过长，将导致滚动体验不佳。委托中的许多元素也会降低滚动性能。简单地移动许多元素需要周期。

To remedy the two latter issues, it is recommended to use `Loader` elements. These can be used to instantiate additional elements when they are needed. For instance, an expanding delegate may use a `Loader` to postpone the instantiation of its detailed view until it is needed. For the same reason, it is good to keep the amount of JavaScript to a minimum in each delegate. It is better to let them call complex pieced of JavaScript that resides outside each delegate. This reduces the time spent compiling JavaScript each time a delegate is created.

要解决这两个问题，建议使用`Loader`元素。当需要时，可以使用它们来实例化其他元素。例如，一个扩展的委托可以使用`Loader`来推迟其详细视图的实例化，直到需要为止。出于同样的原因，最好将每个委托中的JavaScript量保持在最低限度。最好让它们调用位于每个委托之外的复杂JavaScript片段。这减少了每次创建委托时编译JavaScript所花费的时间。

::: tip
Be aware that using a `Loader` to postpone initialization does just that - it postpones a performance issue. This means that the scrolling performance will be improved, but the actual contents will still take time to appear.

请注意，使用`Loader`延迟初始化只是延迟了性能问题。这意味着滚动性能将得到改善，但实际内容仍然需要时间才能出现。
:::

