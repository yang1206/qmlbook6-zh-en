# Browser/HTML vs Qt Quick/QML(浏览器/HTML与Qt Quick/QML）

The browser is the runtime to render HTML and execute the Javascript associated with the HTML. Nowadays modern web applications contain much more JavaScript than HTML. The Javascript inside the browser is a standard ECMAScript environment with some additional browser APIs. A typical JS environment inside the browser has a global object named `window` which is used to interact with the browser window (title, location URL, DOM tree etc.) Browsers provide functions to access DOM nodes by their id, class etc. (which were used by jQuery to provide the CSS selectors) and recently also by CSS selectors (`querySelector`, `querySelectorAll`). Additionally, there is a possibility to call a function after a certain amount of time (`setTimeout`) and to call it repeatedly (`setInterval`). Besides these (and other browser APIs), the environment is similar to QML/JS.

浏览器是渲染HTML并运行时执行与HTML关联的Javascript。如今，现代web应用程序包含的JavaScript比HTML多得多。浏览器中的Javascript是一个标准的ECMAScript环境，带有一些额外的浏览器API。浏览器内的典型JS环境有一个名为window的全局对象，用于与浏览器窗口（标题、位置URL、DOM树等）交互。浏览器提供通过id访问DOM节点的功能，类等（jQuery使用它们来提供CSS选择器），最近CSS选择器（querySelector、querySelectorAll）也使用它们。此外，还可以在一定时间（setTimeout）后调用函数，并重复调用它（setInterval）。除了这些（以及其他浏览器API），环境与QML/JS类似。



Another difference is how JS can appear inside HTML and QML. In HTML, you can execute JS only during the initial page load or in event handlers (e.g. page loaded, mouse pressed). For example, your JS initializes normally on page load, which is comparable to `Component.onCompleted` in QML. By default, you cannot use JS for property bindings in a browser (AngularJS enhances the DOM tree to allow these, but this is far away from standard HTML).

另一个区别是JS在HTML和QML中的显示方式。在HTML中，您只能在初始页面加载期间或在事件处理程序中（例如，页面加载、鼠标按下）执行JS。例如，JS通常在页面加载时初始化，这与QML中Component.onCompleted类似。默认情况下，不能在浏览器中使用JS进行属性绑定（AngularJS增强了DOM树以允许这些绑定，但这与标准HTML相去甚远）。

In QML, JS is a much more of a first-class citizen and is much deeper integrated into the QML render tree. Which makes the syntax much more readable. Besides these differences, people who have developed HTML/JS applications should feel at home using QML/JS.

在QML中，JS是第一公民，并且与QML渲染树集成得更深。这使得语法更加易读。除了这些差异，开发过HTML/JS应用程序的人应该感到使用QML/JS很自在。

