## Creating and Destroying Objects（动态创建和销毁对象）

The `Loader` element makes it possible to populate part of a user interface dynamically. However, the overall structure of the interface is still static. Through JavaScript, it is possible to take one more step and to instantiate QML elements completely dynamically.

`Loader`元素使动态填充用户界面的一部分成为可能。然而，用户界面的整体结构仍然是静态的。通过JavaScript，可以再进一步，完全动态地实例化QML元素。


Before we dive into the details of creating elements dynamically, we need to understand the workflow. When loading a piece of QML from a file or even over the Internet, a component is created. The component encapsulates the interpreted QML code and can be used to create items. This means that loading a piece of QML code and instantiating items from it is a two-stage process. First, the QML code is parsed into a component. Then the component is used to instantiate actual item objects.

在深入探讨动态创建元素的具体细节之前，我们需要理解工作流程。从文件或甚至从互联网加载一段QML代码时，会创建一个组件。该组件封装了解析的QML代码，并可用于创建项目。这意味着加载一段QML代码并从中实例化实际的项目对象是一个两阶段的过程。首先，将QML代码解析为组件。然后使用组件实例化实际的项目对象。

In addition to creating elements from QML code stored in files or on servers, it is also possible to create QML objects directly from text strings containing QML code. The dynamically created items are then treated in a similar fashion once instantiated.

除了从存储在文件或服务器上的QML代码创建元素之外，还可以直接从包含QML代码的文本字符串创建QML对象。一旦实例化，动态创建的项目就会被视为类似的方式。


### Dynamically Loading and Instantiating Items（动态加载和实例化项目）

When loading a piece of QML, it is first interpreted as a component. This includes loading dependencies and validating the code. The location of the QML being loaded can be either a local file, a Qt resource, or even a distance network location specified by a URL. This means that the loading time can be everything from instant, for instance, a Qt resource located in RAM without any non-loaded dependencies, to very long, meaning a piece of code located on a slow server with multiple dependencies that need to be loaded.

加载一段QML时，首先将其解释为组件。这包括加载依赖项并验证代码。正在加载的QML的位置可以是本地文件、Qt资源，甚至是一个URL指定的远程网络位置。这意味着加载时间可以从即时开始，例如，位于RAM中的Qt资源，没有任何未加载的依赖项，到非常长，意味着位于慢服务器上的代码片段，需要加载多个依赖项。

The status of a component being created can be tracked by it is `status` property. The available values are `Component.Null`, `Component.Loading`, `Component.Ready` and `Component.Error`. The `Null` to `Loading` to `Ready` is the usual flow. At any stage, the `status` can change to `Error`. In that case, the component cannot be used to create new object instances. The `Component.errorString()` function can be used to retrieve a user-readable error description.

正在创建的组件的状态可以通过其`status`属性进行跟踪。可用的值有`Component.Null`、`Component.Loading`、`Component.Ready`和`Component.Error`。从`Null`到`Loading`再到`Ready`是正常的流程。在任何阶段，`status`都可以更改为`Error`。在这种情况下，组件不能用于创建新的对象实例。可以使用`Component.errorString()`函数来检索用户可读的错误描述。

When loading components over slow connections, the `progress` property can be of use. It ranges from `0.0`, meaning nothing has been loaded, to `1.0` indicating that all have been loaded. When the component’s `status` changes to `Ready`, the component can be used to instantiate objects. The code below demonstrates how that can be achieved, taking into account the event of the component becoming ready or failing to be created directly, as well as the case where a component is ready slightly later.

在慢速连接上加载组件时，`progress`属性可能会有用。它从`0.0`开始，表示没有加载任何内容，到`1.0`表示已加载所有内容。当组件的`status`更改为`Ready`时，可以使用组件实例化对象。下面的代码演示了如何实现这一点，考虑到组件直接变为就绪或无法创建的事件，以及组件稍后变为就绪的情况。


<<< @/docs/ch15-dynamicqml/src/load-component/create-component.js#M1

The code above is kept in a separate JavaScript source file, referenced from the main QML file.

上面的代码保存在一个单独的JavaScript源文件中，该文件从主QML文件中引用。


<<< @/docs/ch15-dynamicqml/src/load-component/main.qml#M1

The `createObject` function of a component is used to create object instances, as shown above. This not only applies to dynamically loaded components but also `Component` elements inlined in the QML code. The resulting object can be used in the QML scene like any other object. The only difference is that it does not have an `id`.

组件的`createObject`函数用于创建对象实例，如上所示。这不仅适用于动态加载的组件，还适用于QML代码中内联的`Component`元素。生成的对象可以像任何其他对象一样在QML场景中使用。唯一的区别是没有`id`。


The `createObject` function takes two arguments. The first is a `parent` object of the type `Item`. The second is a list of properties and values on the format `{"name": value, "name": value}`. This is demonstrated in the example below. Notice that the properties argument is optional.

`createObject`函数接受两个参数。第一个是`Item`类型的`parent`对象。第二个是格式为`{"name": value, "name": value}`的属性和值列表。如下例所示。注意，属性参数是可选的。


```js
var image = component.createObject(root, {"x": 100, "y": 100});
```

::: tip
A dynamically created component instance is not different to an in-line `Component` element. The in-line `Component` element also provides functions to instantiate objects dynamically.

动态创建的组件实例与内联的`Component`元素没有区别。内联的`Component`元素还提供了动态实例化对象的功能。
:::

#### Incubating Components（孵化组件）

When components are created using `createObject` the creation of the object component is blocking. This means that the instantiation of a complex element may block the main thread, causing a visible glitch. Alternatively, complex components may have to be broken down and loaded in stages using `Loader` elements.

使用`createObject`创建组件时，对象组件的创建是阻塞的。这意味着复杂元素的实例化可能会阻塞主线程，导致可见的故障。或者，复杂组件可能必须使用`Loader`元素分阶段加载。


To resolve this problem, a component can be instantiated using the `incubateObject` method. This might work just as `createObject` and return an instance immediately, or it may call back when the component is ready. Depending on your exact setup, this may or may not be a good way to solve instantiation related animation glitches.

要解决此问题，可以使用`incubateObject`方法实例化组件。这可能与`createObject`一样工作，并立即返回实例，或者当组件准备就绪时回调。根据您的具体设置，这可能是一个很好的方法来解决与实例化相关的动画故障。

To use an incubator, simply use it as `createComponent`. However, the returned object is an incubator and not the object instance itself. When the incubator’s status is `Component.Ready`, the object is available through the `object` property of the incubator. All this is shown in the example below:

要使用孵化器，只需像`createComponent`一样使用它。但是，返回的对象是一个孵化器，而不是对象实例本身。当孵化器的状态为`Component.Ready`时，对象可以通过孵化器的`object`属性获得。如下例所示：

```js
function finishCreate() {
    if (component.status === Component.Ready) {
        var incubator = component.incubateObject(root, {"x": 100, "y": 100});
        if (incubator.status === Component.Ready) {
            var image = incubator.object; // Created at once
        } else {
            incubator.onStatusChanged = function(status) {
                if (status === Component.Ready) {
                    var image = incubator.object; // Created async
                }
            };
        }
    }
}
```

### Dynamically Instantiating Items from Text（从文本中动态实例化项目）

Sometimes, it is convenient to be able to instantiate an object from a text string of QML. If nothing else, it is quicker than putting the code in a separate source file. For this, the `Qt.createQmlObject` function is used.

有时，能够从QML的文本字符串中实例化一个对象是很方便的。如果不是其他原因，那么比将代码放在单独的源文件中要快得多。为此，使用`Qt.createQmlObject`函数。



The function takes three arguments: `qml`, `parent` and `filepath`. The `qml` argument contains the string of QML code to instantiate. The `parent` argument provides a parent object to the newly created object. The `filepath` argument is used when reporting any errors from the creation of the object. The result returned from the function is either a new object or `null`.

函数接受三个参数：`qml`、`parent`和`filepath`。`qml`参数包含要实例化的QML代码的字符串。`parent`参数为新创建的对象提供父对象。`filepath`参数在报告对象的创建错误时使用。函数返回的结果是新的对象或`null`。

::: warning
The `createQmlObject` function always returns immediately. For the function to succeed, all the dependencies of the call must be loaded. This means that if the code passed to the function refers to a non-loaded component, the call will fail and return `null`. To better handle this, the `createComponent` / `createObject` approach must be used.

`createQmlObject`函数总是立即返回。为了使函数成功，所有调用的依赖项都必须加载。这意味着，如果传递给函数的代码引用了一个未加载的组件，调用将失败并返回`null`。为了更好地处理这个问题，必须使用`createComponent`/`createObject`方法。
:::

The objects created using the `Qt.createQmlObject` function resembles any other dynamically created object. That means that it is identical to every other QML object, apart from not having an `id`. In the example below, a new `Rectangle` element is instantiated from in-line QML code when the `root` element has been created.

使用`Qt.createQmlObject`函数创建的对象与任何其他动态创建的对象相似。这意味着它与每个其他QML对象相同，除了没有`id`。在下面的示例中，当`root`元素被创建时，会从内联QML代码中实例化一个新的`Rectangle`元素。

<<< @/docs/ch15-dynamicqml/src/create-object/main.qml#M1

### Managing Dynamically Created Elements（管理动态创建的元素）

Dynamically created objects can be treated as any other object in a QML scene. However, there are some pitfalls that we need to be aware of. The most important is the concept of the creation contexts.

动态创建的对象可以像QML场景中的任何其他对象一样处理。然而，我们需要注意一些陷阱。最重要的概念是创建上下文的概念。


The creation context of a dynamically created object is the context within it is being created. This is not necessarily the same context as the parent exists in. When the creation context is destroyed, so are the bindings concerning the object. This means that it is important to implement the creation of dynamic objects in a place in the code which will be instantiated during the entire lifetime of the objects.

动态创建对象的创建上下文是它被创建的上下文。这不一定与父对象存在的上下文相同。当创建上下文被销毁时，与对象相关的绑定也会被销毁。这意味着在代码中实现动态对象的创建很重要，以确保在对象整个生命周期内实例化。


Dynamically created objects can also be dynamically destroyed. When doing this, there is a rule of thumb: never attempt to destroy an object that you have not created. This also includes elements that you have created, but not using a dynamic mechanism such as `Component.createObject` or `createQmlObject`.

动态创建的对象也可以动态销毁。当这样做时，有一条经验法则：永远不要试图销毁你没有创建的对象。这也包括你创建的元素，但不是使用动态机制，如`Component.createObject`或`createQmlObject`。


An object is destroyed by calling its `destroy` function. The function takes an optional argument which is an integer specifying how many milliseconds the objects shall exist before being destroyed. This is useful too, for instance, let the object complete a final transition.

通过调用其`destroy`函数来销毁对象。该函数接受一个可选参数，该参数是一个整数，指定对象在销毁之前应存在的毫秒数。这对于完成最终过渡很有用。

```js
item = Qt.createQmlObject(...)
...
item.destroy()
```

::: tip
It is possible to destroy an object from within, making it possible to create self-destroying popup windows for instance.

可以从内部销毁一个对象，从而创建出例如自销毁弹出窗口等。
:::

