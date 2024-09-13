# Working with Assets（使用资源）

When working with 3D scenes, the built in meshes quickly grow old. Instead, a good flow from a modelling tool into QML is needed. Qt Quick 3D comes with the Balsam asset import tool, which is used to convert common asset formats into a Qt Quick 3D friendly format.

在处理3D场景时，内置的网格很快就会显得不足。相反，需要有一个从建模工具到QML的良好流程。Qt Quick 3D 自带了 Balsam 资源导入工具，该工具用于将常见的资源格式转换为适合 Qt Quick 3D 使用的格式。



The purpose of Balsam is to make it easy to take assets created in common tools such as [Blender](https://www.blender.org/), Maya or 3ds Max and use them from Qt Quick 3D. Balsam supports the following asset types:

Balsam 的目的是使从 Blender、Maya 或 3ds Max 等常见工具中创建的资源易于在 Qt Quick 3D 中使用。Balsam 支持以下资源类型：

- COLLADA (``*.dae``)
- FBX (``*.fbx``)
- GLTF2 (``*.gltf``, ``*.glb``)
- STL (``*.stl``)
- Wavefront (``*.obj``)

For some format, texture assets can also be exported into a Qt Quick 3D-friendly format, as long as Qt Quick 3D supports the given asset.

对于某些格式，纹理资源也可以导出为适合 Qt Quick 3D 的格式，只要 Qt Quick 3D 支持给定的资源即可。

## Blender（blender）

To generate an asset that we can import, we will use Blender to create a scene with a monkey head in it. We will then export this as a [COLLADA](https://en.wikipedia.org/wiki/COLLADA) file to be able to convert it to a Qt Quick 3D friendly file format using Balsam.

为了生成可以导入的资源，我们将使用 Blender 创建一个包含猴子头的场景。然后将其导出为 COLLADA 文件，以便使用 Balsam 将其转换为适合 Qt Quick 3D 的文件格式。


Blender is available from [https://www.blender.org/](https://www.blender.org/), and mastering Blender is a topic for another book, so we will do the most basic thing possible. Remove the original cube (select the cube with the left mouse button, press``shift + x``, select _Delete_), add a mesh (from the keyboard ``shift + a``, select _Mesh_), and select to add a monkey (select _Monkey_ from the list of available meshes). There are a number of video tutorials demonstrating how to do this. The resulting Blender user interface with the monkey head scene can be seen below.

Blender 可从 https://www.blender.org/ 下载，掌握 Blender 是另一本书的主题，所以我们将做尽可能简单的事情。删除原始立方体（使用左键选择立方体，按 shift + x，选择 Delete），添加网格（从键盘 shift + a，选择 Mesh），然后选择添加猴子（从可用网格列表中选择 Monkey）。有许多视频教程演示了如何做到这一点。可以见下图中的 Blender 用户界面和猴子头场景。


![image](./assets/blender-monkey.png)

Once the head is in place, go to File -> Export -> COLLADA.

一旦头放好，转到文件 -> 导出 -> COLLADA。

![image](./assets/blender-export-menu.png)

This takes you to the Export COLLADA dialog where you can export the resulting scene.

这将带您进入导出 COLLADA 对话框，您可以在其中导出生成的场景。

![image](./assets/blender-export-collada.png)

::: tip Tip
Both the blender scene and the exported COLLADA file (``*.dae``) can be found among the example files for the chapter.

Blender 场景和导出的 COLLADA 文件（*.dae）可以在本章的示例文件中找到。
:::

## Balsam（Balsam）

Once the COLLADA file has been saved to disk, we can prepare it for use in Qt Quick 3D using Balsam. Balsam is available as a command line tool, or through a graphical user interface, using the ``balsamui`` tool. The graphical tool is just a thin layer on top of the command line tool, so there is no difference in what you can do with either tool.

保存 COLLADA 文件后，我们可以使用 Balsam 准备它以在 Qt Quick 3D 中使用。Balsam 作为一个命令行工具提供，也可以通过 ``balsamui`` 工具使用图形用户界面。图形工具只是命令行工具的薄层，所以使用哪种工具没有区别。

We start by adding the ``monkey.dae`` file to the input files section, and set the output folder to a reasonable path.Your paths will most likely be different from the ones shown in the screenshot.

我们首先将 ``monkey.dae`` 文件添加到输入文件部分，并将输出文件夹设置为合理的路径。您的路径可能与屏幕截图中的不同。


![image](./assets/balsamui-1.png)

The _Settings_ tab in the ``balsamui`` includes all the options. These all correspond to a command line option of the ``balsam`` tool. For now, we will leave all of them with their default values.

``balsamui`` 中的 _Settings_ 选项卡包括所有选项。这些都与 ``balsam`` 工具的命令行选项相对应。现在，我们将所有这些选项保留为默认值。


![image](./assets/balsamui-2.png)

Now, go back to the _Input_ tab and click _Convert_. This will result in the following output in the status section of the user interface:

现在，回到 _Input_ 选项卡并点击 _Convert_。这将导致用户界面状态部分出现以下输出：
    
```
Converting 1 files...
[1/1] Successfully converted '/home/.../src/basicasset/monkey.dae'

Successfully converted all files!
```

If you started ``balsamui`` from the command line, you will also see the following output there:

如果您从命令行启动了 ``balsamui``，您还会在命令行中看到以下输出：


```
generated file:  "/home/.../src/basicasset/Monkey.qml"
generated file:  "/home/.../src/basicasset/meshes/suzanne.mesh"
```

This means that Balsam generated a ``*.qml`` file and a ``*.mesh`` file.

这意味着 Balsam 生成了一个 ``*.qml`` 文件和一个 ``*.mesh`` 文件。

## The Qt Quick 3D Assets（Qt Quick 3D 资源）

To use the files from a Qt Quick project we need to add them to the project. This is done in the ``CMakeLists.txt`` file, in the ``qt_add_qml_module`` macro. Add the files to the ``QML_FILES`` and ``RESOURCES`` sections as shown below.

要使用 Qt Quick 项目中的文件，我们需要将它们添加到项目中。这是在 ``CMakeLists.txt`` 文件中完成的，在 ``qt_add_qml_module`` 宏中。将文件添加到 ``QML_FILES`` 和 ``RESOURCES`` 部分如下所示。

```
qt_add_qml_module(appbasicasset
    URI basicasset
    VERSION 1.0
    QML_FILES main.qml Monkey.qml 
    RESOURCES meshes/suzanne.mesh
)
```

Having done this, we can populate a ``View3D`` with the ``Monkey.qml`` as shown below. 

完成此操作后，我们可以使用 ``Monkey.qml`` 填充 ``View3D``，如下所示。

<<< @/docs/ch12-qtquick3d/src/basicasset/main.qml#scene

The ``Monkey.qml`` contains the entire Blender scene, including the camera and a light, so the result is a complete scene as shown below.

``Monkey.qml`` 包含整个 Blender 场景，包括相机和光源，因此结果是一个完整的场景，如下所示。

![image](./assets/asset-first-input.png)

The interested reader is encouraged to explore the ``Monkey.qml`` file. As you will see, it contains a completely normal Qt Quick 3D scene built from the elements we already have used in this chapter.

鼓励感兴趣的读者探索 ``Monkey.qml`` 文件。如您所见，它包含一个完全正常的 Qt Quick 3D 场景，由我们在本章中已经使用的元素构建而成。

::: tip Tip
As ``Monkey.qml`` is generated by a tool, do not modify the file manually. If you do, your changes will be overwritten if you ever have to regenerate the file using Balsam.

``Monkey.qml`` 是由工具生成的，不要手动修改文件。如果你这样做，如果你必须使用 Balsam 重新生成文件，你的更改将被覆盖。
:::

An alternative to using the entire scene from Blender is to use the ``*.mesh`` file in a Qt Quick 3D scene. This is demonstrated in the code below.

使用 Blender 的整个场景的另一种选择是在 Qt Quick 3D 场景中使用 ``*.mesh`` 文件。如下面的代码所示。


Here, we put a ``DirectionalLight`` and ``PerspectiveCamera`` into a ``View3D``, and combine it with the mesh using a ``Model`` element. This way, we can control the positioning, scale, and lighting from QML. We also specify a different, yellow, material for the monkey head.

在这里，我们将 ``DirectionalLight`` 和 ``PerspectiveCamera`` 放入 ``View3D`` 中，并使用 ``Model`` 元素将其与网格结合。这样，我们可以从 QML 中控制定位、缩放和照明。我们还为猴子头部指定了一个不同的、黄色的材质。

<<< @/docs/ch12-qtquick3d/src/basicasset/main.qml#mesh

The resulting view is shown below.

结果视图如下所示。

![image](./assets/asset-second-input.png)

This demonstrates how a simple mesh can be exported from a 3D design tool such as blender, converted to a Qt Quick 3D format and then used from QML. One thing to think about is that we can import the entire scene as is, i.e. using ``Monkey.qml``, or use only the assets, e.g. ``suzanne.mesh``. This puts you in control of the trade-off between simple importing of a scene, and added complexity while gaining flexibility by setting up the scene in QML.

这演示了如何从像 Blender 这样的 3D 设计工具中导出一个简单的网格，将其转换为 Qt Quick 3D 格式，然后从 QML 中使用它。需要考虑的一件事是，我们可以导入整个场景，即使用 ``Monkey.qml``，或者只使用资产，例如 ``suzanne.mesh``。这使您能够在简单导入场景和增加复杂性之间进行权衡，同时通过在 QML 中设置场景来获得灵活性。


