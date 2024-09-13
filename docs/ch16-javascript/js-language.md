# JS Language

This chapter will not give you a general introduction to JavaScript. There are other books out there for a general introduction to JavaScript, please visit this great side on [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript).

​本章不会对JavaScript进行一般性介绍。关于JavaScript的一般介绍，还有其他书籍，请访问[开发者网](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript).
并查看。

On the surface JavaScript is a very common language and does not differ a lot from other languages:

从表面上看，JavaScript是一种非常常见的语言，与其他语言没有太大区别：

```js
function countDown() {
  for(var i=0; i<10; i++) {
    console.log('index: ' + i)
  }
}

function countDown2() {
  var i=10;
  while( i>0 ) {
    i--;
  }
}
```

But be warned JS has function scope and not block scope as in C++ (see [Functions and function scope](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Functions_and_function_scope)).

请注意，JS具有函数作用域，而不是C++中的块作用域（请参阅函数和函数作用域）。


The statements `if ... else`, `break`, `continue` also work as expected. The switch case can also compare other types and not just integer values:

`if ... else`，`break`，`continue`语句也像预期的那样工作。switch case也可以比较其他类型，而不仅仅是整数值：

```js
function getAge(name) {
  // switch over a string
  switch(name) {
  case "father":
    return 58;
  case "mother":
    return 56;
  }
  return unknown;
}
```

JS knows several values which can be false, e.g. `false`, `0`, `""`, `undefined`, `null`). For example, a function returns by default `undefined`. To test for false use the `===` identity operator. The `==` equality operator will do type conversion to test for equality. If possible use the faster and better `===` strict equality operator which will test for identity (see [Comparison operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)).

JS知道几个可以返回false的值，例如`false`，`0`，`""`，`undefined`，`null`）。例如，函数默认返回`undefined`。要测试false，请使用`===`身份运算符。`==`相等运算符将进行类型转换以测试相等性。如果可能，请使用更快且更好的`===`严格相等运算符，该运算符将测试身份（请参阅比较运算符）。

Under the hood, javascript has its own ways of doing things. For example arrays:

在引擎盖下，javascript有自己的做事方式。例如数组：

```js
function doIt() {
  var a = [] // empty arrays
  a.push(10) // addend number on arrays
  a.push("Monkey") // append string on arrays
  console.log(a.length) // prints 2
  a[0] // returns 10
  a[1] // returns Monkey
  a[2] // returns undefined
  a[99] = "String" // a valid assignment
  console.log(a.length) // prints 100
  a[98] // contains the value undefined
}
```

Also for people coming from C++ or Java which are used to an OO language JS just works differently. JS is not purely an OO language it is a so-called prototype based language. Each object has a prototype object. An object is created based on his prototype object. Please read more about this in the book [Javascript the Good Parts by Douglas Crockford](http://javascript.crockford.com).

对于习惯了面向对象语言（如C++或Java）的人来说，JS的工作方式有所不同。JS不仅仅是一种纯面向对象的语言，而是一种所谓的基于原型的语言。每个对象都有一个原型对象。对象是根据其原型对象创建的。请阅读Douglas Crockford的《JavaScript的精华》一书以了解更多信息。



To test some small JS snippets you can use the online [JS Console](http://jsconsole.com) or just build a little piece of QML code:

要测试一些小的JS片段，可以使用在线JS控制台，或者构建一些小的QML代码：

```qml
import QtQuick 2.5

Item {
  function runJS() {
    console.log("Your JS code goes here");
  }
  Component.onCompleted: {
    runJS();
  }
}
```

