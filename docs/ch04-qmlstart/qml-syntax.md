# QML Syntax(QML语法)

QML is a declarative language used to describe how objects relate to each other. QtQuick is a framework built on QML for building the user interface of your application. It breaks down the user interface into smaller elements, which can be combined into components. QtQuick describes the look and the behavior of these user interface elements. This user interface description can be enriched with JavaScript code to provide simple but also more complex logic. In this perspective, it follows the HTML-JavaScript pattern but QML and QtQuick are designed from the ground up to describe user interfaces, not text-documents.

QML 是一种声明性语言，用于描述对象之间的关系。QtQuick 是建立在 QML 上的一个框架，用于构建应用程序的用户界面。它将用户界面分解为较小的元素，这些元素可以组合成组件。QtQuick 描述了这些用户界面元素的外观和行为。这种用户界面描述可以通过 JavaScript 代码来丰富，以提供简单或复杂的逻辑。在这方面，它遵循了 HTML-JavaScript 的模式，但 QML 和 QtQuick 是从头开始设计的，目的是描述用户界面，而不是文本文档。

In its simplest form, QtQuick lets you create a hierarchy of elements. Child elements inherit the coordinate system from the parent. An `x,y` coordinate is always relative to the parent.

最简单的形式，QtQuick 允许你创建一个元素层次结构。子元素从父元素继承坐标系统。`x,y` 坐标始终相对于父元素。

::: tip
QtQuick builds on QML. The QML language only knows of elements, properties, signals and bindings. QtQuick is a framework built on QML. Using default properties, the hierarchy of QtQuick elements can be constructed in an elegant way.

QtQuick 建立在 QML 之上。QML 语言只知道元素、属性、信号和绑定。QtQuick 是建立在 QML 之上的一个框架。使用默认属性，QtQuick 元素的层次结构可以以一种优雅的方式构建。
:::

![](./assets/scene.png)

Let’s start with a simple example of a QML file to explain the different syntax.

让我们从一个简单的 QML 文件示例开始，解释不同的语法。

<<< @/docs/ch04-qmlstart/src/concepts/RectangleExample.qml#global

* The `import` statement imports a module. An optional version in the form of `<major>.<minor>` can be added.
* 这个 `import` 声明导入了一个模块，可以添加一个可选的 `<major>.<minor>` 形式的版本。
* Comments can be made using `//` for single line comments or `/* */` for multi-line comments. Just like in C/C++ and JavaScript  
* 注释可以使用 `//` 进行单行注释，或 `/* */` 进行多行注释。就像在 C/C++ 和 JavaScript 中一样
* Every QML file needs to have exactly one root element, like HTML
* 每个 QML 文件都需要有一个根元素，就像 HTML 一样
* An element is declared by its type followed by `{ }`
* 元素通过其类型声明，后跟 `{ }`
* Elements can have properties, they are in the form `name: value`
* 元素可以有属性，它们的形式是 `name: value`
* Arbitrary elements inside a QML document can be accessed by using their `id` (an unquoted identifier)
* 可以使用 `id`（未引用的标识符）访问 QML 文档中的任意元素
* Elements can be nested, meaning a parent element can have child elements. The parent element can be accessed using the `parent` keyword
* 元素可以嵌套，这意味着父元素可以有子元素。可以使用 `parent` 关键字访问父元素

With the `import` statement you import a QML module by name. In Qt5 you had to specify a major and minor version (e.g. `2.15`), this is now optional in Qt6. For the book content we drop this optional version number as normally you automatically want to choose the newest version available from your selected Qt Kit.

使用 `import` 语句按名称导入一个 QML 模块。在 Qt5 中，你必须指定一个主要和次要版本（例如 `2.15`），这在 Qt6 中现在是可选的。对于本书内容，我们省略了这个可选的版本号，因为通常你总是想选择你选择的 Qt Kit 中可用的最新版本。


::: tip
Often you want to access a particular element by id or a parent element using the `parent` keyword. So it’s good practice to name your root element “root” using `id: root`. Then you don’t have to think about how the root element is named in your QML document.

通常，你希望通过 id 或使用 `parent` 关键字访问父元素来访问特定元素。因此，使用 `id: root` 命名你的根元素是一个好习惯。然后你就不必考虑你的 QML 文档中根元素是如何命名的了。
:::

::: tip
You can run the example using the Qt Quick runtime from the command line from your OS like this:

你可以像这样从你的操作系统使用 Qt Quick 运行时从命令行运行示例：

    $ $QTDIR/bin/qml RectangleExample.qml

Where you need to replace the `$QTDIR` to the path to your Qt installation. The `qml` executable initializes the Qt Quick runtime and interprets the provided QML file.

你需要将 `$QTDIR` 替换为你的 Qt 安装路径。`qml` 可执行文件初始化 Qt Quick 运行时并解释提供的 QML 文件。

In Qt Creator, you can open the corresponding project file and run the document  `RectangleExample.qml`.

在 Qt Creator 中，你可以打开相应的项目文件并运行文档 `RectangleExample.qml`。
:::

## Properties

Elements are declared by using their element name but are defined by using their properties or by creating custom properties. A property is a simple key-value pair, e.g. `width: 100`, `text: 'Greetings'`, `color: '#FF0000'`. A property has a well-defined type and can have an initial value.

元素通过使用其元素名称来声明，但通过使用其属性或创建自定义属性来定义。属性是一个简单的键值对，例如 `width: 100`，`text: 'Greetings'`，`color: '#FF0000'`。属性有一个定义良好的类型，并且可以有一个初始值。




```qml
Text {
    // (1) identifier
    id: thisLabel

    // (2) set x- and y-position
    x: 24; y: 16

    // (3) bind height to 2 * width
    height: 2 * width

    // (4) custom property
    property int times: 24

    // (5) property alias
    property alias anotherTimes: thisLabel.times

    // (6) set text appended by value
    text: "Greetings " + times

    // (7) font is a grouped property
    font.family: "Ubuntu"
    font.pixelSize: 24

    // (8) KeyNavigation is an attached property
    KeyNavigation.tab: otherLabel

    // (9) signal handler for property changes
    onHeightChanged: console.log('height:', height)

    // focus is need to receive key events
    focus: true

    // change color based on focus value
    color: focus ? "red" : "black"
}
```

Let’s go through the different features of properties:

让我们来看看属性的不同特性：


* **(1)** `id` is a very special property-like value, it is used to reference elements inside a QML file (called “document” in QML). The `id` is not a string type but rather an identifier and part of the QML syntax. An `id` needs to be unique inside a document and it can’t be reset to a different value, nor may it be queried. (It behaves much like a reference in the C++ world.)

* **(1)** `id` 是一个非常特殊的属性值，它用于引用 QML 文件中的元素（在 QML 中称为“文档”）。`id` 不是字符串类型，而是 QML 语法的标识符的一部分。`id` 在文档中需要是唯一的，不能重置为不同的值，也不能查询。（它就像 C++ 世界中的引用一样。）


* **(2)** A property can be set to a value, depending on its type. If no value is given for a property, an initial value will be chosen. You need to consult the documentation of the particular element for more information about the initial value of a property.

* **(2)** 属性可以设置为某个值，具体取决于其类型。如果没有为属性指定值，将选择一个初始值。你需要查阅特定元素的文档，以获取有关属性的初始值的更多信息。

* **(3)** A property can depend on one or many other properties. This is called *binding*. A bound property is updated when its dependent properties change. It works like a contract, in this case, the `height` should always be two times the `width`.

* **(3)** 属性可以依赖于一个或多个其他属性。这称为*绑定*。当其依赖属性发生变化时，绑定属性会更新。它就像一个合同，在这种情况下，`height` 应该总是 `width` 的两倍。


* **(4)** Adding new properties to an element is done using the `property` qualifier followed by the type, the name and the optional initial value (`property <type> <name> : <value>`). If no initial value is given, a default initial value is chosen.

* **(4)** 使用 `property` 限定符后跟类型、名称和可选的初始值（`property <type> <name> : <value>`）来为元素添加新属性。如果没有给出初始值，将选择一个默认初始值。
::: tip
You can also declare one property to be the default property using `default` keyword. If another element is created inside the element and not explicitly bound to a property, it is bound to the default property. For instance, This is used when you add child elements. The child elements are added automatically to the default property `children` of type list if they are visible elements.

你也可以使用 `default` 关键字声明一个属性为默认属性。如果元素内部创建了另一个元素，并且没有明确绑定到属性，则它将绑定到默认属性。例如，当你添加子元素时，如果它们是可见元素，子元素将自动添加到类型为列表的默认属性 `children` 中。
:::

* **(5)** Another important way of declaring properties is using the `alias` keyword (`property alias <name>: <reference>`). The `alias` keyword allows us to forward a property of an object or an object itself from within the type to an outer scope. We will use this technique later when defining components to export the inner properties or element ids to the root level. A property alias does not need a type, it uses the type of the referenced property or object.

* **(5)** 另一种声明属性的方法是使用 `alias` 关键字（`property alias <name>: <reference>`）。`alias` 关键字允许我们将对象或对象本身的属性从类型内部转发到外部作用域。我们将在定义组件时使用这种技术，以将内部属性或元素 id 导出为根级别。属性别名不需要类型，它使用引用属性或对象的类型。

* **(6)** The `text` property depends on the custom property `times` of type int. The `int` based value is automatically converted to a `string` type. The expression itself is another example of binding and results in the text being updated every time the `times` property changes.

* **(6)** `text` 属性依赖于类型为 `int` 的自定义属性 `times`。基于 `int` 的值会自动转换为 `string` 类型。表达式本身是绑定的一个例子，每次 `times` 属性发生变化时，文本都会更新。


* **(7)** Some properties are grouped properties. This feature is used when a property is more structured and related properties should be grouped together. Another way of writing grouped properties is `font { family: "Ubuntu"; pixelSize: 24 }`.

* **(7)** 有些属性是分组属性。当属性更结构化且相关属性应分组在一起时，会使用此功能。另一种编写分组属性的方法是 `font { family: "Ubuntu"; pixelSize: 24 }`。

* **(8)** Some properties belong to the element class itself. This is done for global settings elements which appear only once in the application (e.g. keyboard input). The writing is `<Element>.<property>: <value>`.

* **(8)** 有些属性属于元素类本身。这是为仅在应用程序中出现的全局设置元素（例如键盘输入）而做的。写法是 `<Element>.<property>: <value>`。

* **(9)** For every property, you can provide a signal handler. This handler is called after the property changes. For example, here we want to be notified whenever the height changes and use the built-in console to log a message to the system.

* **(9)** 对于每个属性，你可以提供一个信号处理程序。该处理程序在属性更改后调用。例如，我们希望在高度更改时收到通知，并使用内置的控制台将消息记录到系统中。

::: warning
An element id should only be used to reference elements inside your document (e.g. the current file). QML provides a mechanism called "dynamic scoping", where documents loaded later on overwrite the element IDs from documents loaded earlier. This makes it possible to reference element IDs from previously loaded documents if they have not yet been overwritten. It’s like creating global variables. Unfortunately, this frequently leads to really bad code in practice, where the program depends on the order of execution. Unfortunately, this can’t be turned off. Please only use this with care; or, even better, don’t use this mechanism at all. It’s better to export the element you want to provide to the outside world using properties on the root element of your document.

元素 id 应该只用于引用文档内部的元素（例如当前文件）。QML 提供了一种称为“动态作用域”的机制，其中稍后加载的文档会覆盖先前加载的文档中的元素 ID。如果尚未覆盖，则可以引用先前加载的文档中的元素 ID。这就像创建全局变量一样。不幸的是，这通常会导致程序严重依赖于执行顺序。不幸的是，这是无法关闭的。请谨慎使用此机制；或者，更好的是，根本不使用此机制。最好使用文档根元素上的属性将你想要提供给外部世界的元素导出。
:::

## Scripting(脚本)

QML and JavaScript (also known as ECMAScript) are best friends. In the *JavaScript* chapter we will go into more detail on this symbiosis. Currently, we just want to make you aware of this relationship.

QML和JavaScript（也称为ECMAScript）是最好的朋友。在*JavaScript*章节中，我们将更详细地讨论这种共生关系。目前，我们只是想让你意识到这种关系。

<<< @/docs/ch04-qmlstart/src/concepts/ScriptingExample.qml#text

* **(1)** The text changed handler `onTextChanged` prints the current text every time the text changed due to the space bar being pressed. As we use a parameter injected by the signal, we need to use the function syntax here. It's also possible to use an arrow function (`(text) => {}`), but we feel `function(text) {}` is more readable.

* **(1)** 每当按下空格键导致文本更改时，文本更改处理程序 `onTextChanged` 都会打印当前文本。由于我们使用了信号注入的参数，因此这里需要使用函数语法。也可以使用箭头函数（`(text) => {}`），但我们觉得 `function(text) {}` 更易读。


* **(2)** When the text element receives the space key (because the user pressed the space bar on the keyboard) we call a JavaScript function `increment()`.

* **(2)** 当文本元素接收到空格键（因为用户在键盘上按下了空格键）时，我们调用一个JavaScript函数 `increment()`。


* **(3)** Definition of a JavaScript function in the form of `function <name>(<parameters>) { ... }`, which increments our counter `spacePresses`. Every time `spacePresses` is incremented, bound properties will also be updated.

* **(3)** 以 `function <name>(<parameters>) { ... }` 形式定义JavaScript函数，该函数递增我们的计数器 `spacePresses`。每次递增 `spacePresses` 时，绑定属性也会更新。

## Binding(绑定)

The difference between the QML `:` (binding) and the JavaScript `=` (assignment) is that the binding is a contract and keeps true over the lifetime of the binding, whereas the JavaScript assignment (`=`) is a one time value assignment.

QML `:`（绑定）和JavaScript `=`（赋值）之间的区别在于，绑定是一个合同，在绑定生命周期内始终保持不变，而JavaScript赋值（`=`）是一次性值赋值。


The lifetime of a binding ends when a new binding is set on the property or even when a JavaScript value is assigned to the property. For example, a key handler setting the text property to an empty string would destroy our increment display:

绑定的生命周期在设置新绑定或在属性上分配JavaScript值时结束。例如，将文本属性设置为空字符串的键处理程序将破坏我们的增量显示：

<<< @/docs/ch04-qmlstart/src/concepts/ScriptingExample.qml#clear-binding{2}


After pressing escape, pressing the space bar will not update the display anymore, as the previous binding of the `text` property (*text: “Space pressed: ” + spacePresses + ” times”*) was destroyed.

按下 `Esc` 后，按下空格键将不再更新显示，因为 `text` 属性的先前绑定（*text: “Space pressed: ” + spacePresses + ” times”*）已被破坏。

When you have conflicting strategies to change a property as in this case (text updated by a change to a property increment via a binding and text cleared by a JavaScript assignment) then you can’t use bindings! You need to use assignment on both property change paths as the binding will be destroyed by the assignment (broken contract!).

在这种情况下，当您需要更改属性时（通过绑定更新属性增量，并通过JavaScript赋值清除文本），则不能使用绑定！您需要在两个属性更改路径上使用赋值，因为绑定将被赋值破坏（合同破裂！）。

