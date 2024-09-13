# Introduction to Controls( 控件介绍 )

Using Qt Quick from scratch gives you primitive graphical and interaction elements from which you can build your user interfaces. Using Qt Quick Controls you start from a slightly more structured set of controls to build from. 

从头开始使用Qt Quick，你可以从原始的图形和交互元素开始构建用户界面。使用Qt Quick Controls，你可以从稍微更结构化的控件集合开始构建。

The controls range from simple text labels and buttons to more complex ones such as sliders and dials. These elements are handy if you want to create a user interface based on classic interaction patterns, as they provide a good foundation to stand on.

这些控件从简单的文本标签和按钮到更复杂的控件，如滑块和转盘。如果你想要创建一个基于经典交互模式的用户界面，这些元素是非常有用的，因为它们提供了一个很好的基础。


Qt Quick Controls come with a number of styles out of the box that are shown in the table below. The *Basic* style is a basic flat style. The *Universal* style is based on the Microsoft Universal Design Guidelines, while *Material* is based on Google’s Material Design Guidelines, and the *Fusion* style is a desktop-oriented style.

Qt Quick Controls提供了许多开箱即用的样式，如表所示。*Basic*样式是一种基本的扁平样式。*Universal*样式基于Microsoft Universal Design Guidelines，*Material*样式基于Google的Material Design Guidelines，*Fusion*样式是一种面向桌面的样式。


Some of the styles can be tweaked by modifying palettes. The *Imagine* style is based on image assets, this allows a graphical designer to create a new style without writing any code at all, not even for palette colour codes.

一些样式可以通过修改调色板进行调整。*Imagine*样式基于图像资源，这允许图形设计师创建一个新的样式，而不需要编写任何代码，甚至不需要为调色板颜色代码编写代码。

* Basic
    
    ![](./assets/style-basic.png)

* Fusion

    ![](././assets/style-fusion.png)

* macOS

    ![](././assets/style-imagine.png)

* Material

    ![](././assets/style-material.png)

* Imagine

    ![](././assets/style-imagine.png)

* Windows

    ![](././assets/style-imagine.png)
    
* Universal
    
    ![](./assets/style-universal.png)

Qt Quick Controls 2 is available from the `QtQuick.Controls` import. The following modules are also of interest:

Qt Quick Controls 2可以从`QtQuick.Controls`导入。着重介绍一下模块：

* **`QtQuick.Controls`** - The basic controls.(基本控件)
* **`QtQuick.Templates`** - Provides the behavioral, non-visual base types for the controls.(提供控件的行为、非可视基类型)
* **`QtQuick.Controls.Imagine`** - Imagine style theming support.(Imagine样式主题支持)
* **`QtQuick.Controls.Material`** - Material style theming support.(Material样式主题支持)
* **`QtQuick.Controls.Universal`** - Universal style theming support.(Universal样式主题支持)
* **`Qt.labs.platform`** - Support for platform native dialogs for common tasks such as picking files, colours, etc, as well as system tray icons and standard paths.(支持平台原生对话框，用于常见任务，如选择文件、颜色等，以及系统托盘图标和标准路径。)

:::warning Qt.Labs
Notice that the `Qt.labs` modules are experimental, meaning that their APIs can have breaking changes between Qt versions.

请注意，`Qt.labs`模块是实验性的，这意味着它们的API可以在Qt版本之间发生重大变化。
:::

