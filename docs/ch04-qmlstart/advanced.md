# Advanced Techniques(高级技术)

## Performance of QML(性能)

QML and Javascript are interpreted languages. This means that they do not have to be processed by a compiler before being executed. Instead, they are being run inside an execution engine. However, as interpretation is a costly operation, various techniques are used to improve performance.

QML 和 Javascript 都是解释型语言。这意味着它们在执行前无需经过编译器处理。相反，它们是在执行引擎内运行的。然而，由于解释是一项成本高昂的操作，因此需要使用各种技术来提高性能。


The QML engine uses just-in-time (JIT) compilation to improve performance. It also caches the intermediate output to avoid having to recompile. This works seamlessly for you as a developer. The only trace of this is that files ending with `qmlc` and `jsc` can be found next to the source files.

QML引擎使用即时（JIT）编译来提高性能。它还缓存中间输出，以避免重新编译。作为开发人员，您无需为此做任何工作。唯一的痕迹是可以在源文件旁边找到以 `qmlc` 和 `jsc` 结尾的文件。

If you want to avoid the initial start-up penalty induced by the initial parsing you can also pre-compile your QML and Javascript. This requires you to put your code into a Qt resource file, and is described in detail in the [Compiling QML Ahead of Time](https://doc.qt.io/qt-6/qtquick-deployment.html#ahead-of-time-compilation) chapter in the Qt documentation.

如果您想避免由初始解析引起的初始启动惩罚，也可以预先编译您的QML和Javascript。这要求您将代码放入Qt资源文件中，并在Qt文档中的 [提前编译QML](https://doc.qt.io/qt-6/qtquick-deployment.html#ahead-of-time-compilation) 章节中进行了详细描述。

