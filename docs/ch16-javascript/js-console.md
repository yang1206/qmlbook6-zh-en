# Creating a JS Console（创建一个JS控制台）

As a little example, we will create a JS console. We need an input field where the user can enter his JS expressions and ideally there should be a list of output results. As this should more look like a desktop application we use the Qt Quick Controls module.

作为一个小例子，我们将创建一个JS控制台。我们需要一个输入字段，用户可以在其中输入他的JS表达式，理想情况下应该有一个输出结果列表。由于这看起来更像一个桌面应用程序，我们使用Qt Quick Controls模块。

::: tip
A JS console inside your next project can be really beneficial for testing. Enhanced with a Quake-Terminal effect it is also good to impress customers. To use it wisely you need to control the scope the JS console evaluates in, e.g. the currently visible screen, the main data model, a singleton core object or all together.

下一个项目中的JS控制台对测试非常有益。通过Quake-Terminal效果增强，这也有助于给客户留下深刻印象。要明智地使用它，您需要控制JS控制台计算的范围，例如当前可见的屏幕、主数据模型、单一核心对象或所有这些。
:::



![image](./assets/jsconsole.png)

We use Qt Creator to create a Qt Quick UI project using Qt Quick controls. We call the project JSConsole. After the wizard has finished we have already a basic structure for the application with an application window and a menu to exit the application.

我们使用Qt Creator创建一个使用Qt Quick Controls的Qt Quick UI项目。我们称这个项目为JSConsole。在向导完成后，我们已经有了一个基本的应用程序结构，包括一个应用程序窗口和一个退出应用程序的菜单。



For the input, we use a TextField and a Button to send the input for evaluation. The result of the expression evaluation is displayed using a ListView with a ListModel as the model and two labels to display the expression and the evaluated result.


对于输入，我们使用TextField和Button来发送要评估的输入。表达式评估的结果使用ListView和ListModel作为模型，以及两个标签来显示表达式和评估结果。


Our application will be split in two files: 

我们的应用程序将分为两个文件：

* `JSConsole.qml`: the main view of the app（应用程序的主视图）
* `jsconsole.js`: the javascript library responsible for evaluating user statements（负责评估用户语句的javascript库）


## JSConsole.qml

### Application window
<<< @/docs/ch16-javascript/src/JSConsole/JSConsole.qml#application-window

### Form

<<< @/docs/ch16-javascript/src/JSConsole/JSConsole.qml#form{12,19,40-42,45,50,54-55}

### Calling the library

The evaluation function `jsCall` does the evaluation not by itself this has been moved to a JS module (`jsconsole.js`) for clearer separation.

评估函数`jsCall`本身并不进行评估，这已经被移动到JS模块（`jsconsole.js`）中，以便更清晰的分离。

<<< @/docs/ch16-javascript/src/JSConsole/JSConsole.qml#import

<<< @/docs/ch16-javascript/src/JSConsole/JSConsole.qml#js-call

::: tip
For safety, we do not use the `eval` function from JS as this would allow the user to modify the local scope. We use the Function constructor to create a JS function on runtime and pass in our scope as this variable. As the function is created every time it does not act as a closure and stores its own scope, we need to use `this.a = 10` to store the value inside this scope of the function. This scope is set by the script to the scope variable.

为了安全起见，我们不使用JS中的`eval`函数，因为这将允许用户修改本地范围。我们使用Function构造函数在运行时创建一个JS函数，并将作用域变量作为this变量传递。由于每次创建函数，它并不作为闭包工作，并存储自己的作用域，我们需要使用`this.a = 10`来存储函数作用域中的值。这个作用域由脚本设置为作用域变量。
:::

## jsconsole.js

<<< @/docs/ch16-javascript/src/JSConsole/jsconsole.js#global

The data return from the call function is a JS object with a result, expression and error property: `data: { expression: "", result: "", error: "" }`. We can use this JS object directly inside the ListModel and access it then from the delegate, e.g. `delegate.model.expression` gives us the input expression.

调用函数返回的数据是一个JS对象，具有结果、表达式和错误属性：`data: { expression: "", result: "", error: "" }`。我们可以直接在ListModel中使用这个JS对象，然后从委托中访问它，例如`delegate.model.expression`给我们输入的表达式。
