# JavaScript(javascript)

JavaScript is the lingua-franca on web client development. It also starts to get traction on web server development mainly by node js. As such it is a well-suited addition as an imperative language onto the side of declarative QML language. QML itself as a declarative language is used to express the user interface hierarchy but is limited to express operational code. Sometimes you need a way to express operations, here JavaScript comes into play.

JavaScript是web客户端开发的通用语言。它也开始主要通过node js在web服务器开发上获得吸引力。因此，它非常适合作为命令式语言添加到声明式QML语言中。QML本身作为一种声明性语言，用于表示用户界面层次结构，但仅限于表示操作代码。有时你需要一种表达操作的方式，这里JavaScript发挥了作用。



::: tip
There is an open question in the Qt community about the right mixture about QML/JS/Qt C++ in a modern Qt application. The commonly agreed recommended mixture is to limit the JS part of your application to a minimum and do your business logic inside Qt C++ and the UI logic inside QML/JS.

在Qt社区中，关于现代Qt应用程序中QML/JS/Qt C++的正确混合比例存在一个开放问题。普遍同意的推荐混合比例是将应用程序的JS部分限制在最小范围内，并在Qt C++中执行业务逻辑，在QML/JS中执行UI逻辑。
:::

This book pushes the boundaries, which is not always the right mix for a product development and not for everyone. It is important to follow your team skills and your personal taste. In doubt follow the recommendation.

本书推动了边界，这并不总是产品开发或每个人的正确混合比例。重要的是要遵循团队技能和个人品味。如有疑问，请遵循建议。

Here a short example of how JS used in QML looks like:

下面是一个简单的例子，展示了在QML中使用JS的方式：

```qml
Button {
  width: 200
  height: 300
  property bool checked: false
  text: "Click to toggle"

  // JS function
  function doToggle() {
    checked = !checked
  }

  onClicked: {
    // this is also JavaScript
    doToggle();
    console.log('checked: ' + checked)
  }
}
```

So JavaScript can come in many places inside QML as a standalone JS function, as a JS module and it can be on every right side of a property binding.

因此，JavaScript可以以独立JS函数、JS模块的形式出现在QML中的任何地方，也可以出现在属性绑定的任何右侧。

```qml
import "util.js" as Util // import a pure JS module

Button {
  width: 200
  height: width*2 // JS on the right side of property binding

  // standalone function (not really useful)
  function log(msg) {
    console.log("Button> " + msg);
  }

  onClicked: {
    // this is JavaScript
    log();
    Qt.quit();
  }
}
```

Within QML you declare the user interface, with JavaScript you make it functional. So how much JavaScript should you write? It depends on your style and how familiar you are with JS development. JS is a loosely typed language, which makes it difficult to spot type defects. Also, functions expect all argument variations, which can be a very nasty bug to spot. The way to spot defects is rigorous unit testing or acceptance testing. So if you develop real logic (not some glue lines of code) in JS you should really start using the test-first approach. In generally mixed teams (Qt/C++ and QML/JS) are very successful when they minimize the amount of JS in the frontend as the domain logic and do the heavy lifting in Qt C++ in the backend. The backend should then be rigorous unit tested so that the frontend developers can trust the code and focus on all these little user interface requirements.

在QML中声明用户界面，使用JavaScript使其功能化。那么你应该写多少JavaScript呢？这取决于你的风格以及你对JS开发的熟悉程度。JS是一种松散类型的语言，因此很难发现类型缺陷。此外，函数需要所有参数的变化，这可能是一个非常讨厌的错误。发现缺陷的方法是严格的单元测试或验收测试。因此，如果您在JS中开发真正的逻辑（而不是一些粘合代码行），您应该真正开始使用测试优先的方法。在一般混合团队（Qt/C++和QML/JS）中，当他们最小化前端的JS作为域逻辑，并重点放在后端进行Qt C++的实现时，非常成功。后端应该经过严格的单元测试，这样前端开发人员就可以信任代码并专注于所有这些小的用户界面需求。


::: tip
In general: backend developers are functional driven and frontend developers are user story driven.

一般来说：后端开发人员是功能驱动的，前端开发人员是用户故事驱动的。
:::

