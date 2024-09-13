# Animations(动画)

There are multiple ways to add animations to Qt Quick 3D scenes. The most basic one is to move, rotate, and scale ``Model`` elements in the scene. However, in many cases, we want to modify the actual meshes. There are two basic approaches to this: morphing animations and skeletal animations.

有多种方式可以为 Qt Quick 3D 场景添加动画。最基本的一种是在场景中移动、旋转和缩放 ``Model`` 元素。然而，在很多情况下，我们希望修改实际的网格。对此有两种基本的方法：变形动画（morphing animations）和骨骼动画（skeletal animations）。


Morphing animations lets you create a number of target shapes to which various weights can be assigned. By combining the target shapes based on the weights, a deformed, i.e. morphed, shape is produced. This is commonly used to simulate deformations of soft materials.

变形动画（morphing animations）允许你创建多个目标形状，并为它们分配不同的权重。通过根据权重组合目标形状，可以生成变形的形状（即变形的形状）。这通常用于模拟软材料的变形。

Skeletal animations is used to pose an object, such as a body, based on the positions of a skeleton made of bones. These bones will affect the body, thus deform it into the pose required.

骨骼动画（skeletal animations）用于根据骨骼的姿势来定位对象，例如身体。这些骨骼会影响身体，从而将其变形为所需的姿势。


For both types of animations, the most common approach is to define the morphing target shapes and bones in a modelling tool, and then export it to QML using the _Balsam_ tool. In this chapter we will do just this for a skeletal animation, but the approach is similar for a morphing animation.

对于这两种类型的动画，最常见的方法是在建模工具中定义变形目标形状和骨骼，然后使用 _Balsam_ 工具将其导出到 QML。在本章中，我们将为此进行骨骼动画，但对于变形动画，方法类似。

# Skeletal Animations（骨骼动画）

The goal of this example is to make Suzanne, the Blender monkey head, wave with one of her ears.

本例的目标是让 Blender Suzanness的耳朵摆动。

![image](./assets/monkey.gif)

Skeletal animation is sometimes known as vertex skinning. Basically, a skeleton is put inside of a mesh and the vertexes of the mesh are bound to the skeleton. This way, when moving the skeleton, the mesh is deformed into various poses.

骨骼动画有时被称为顶点蒙皮。基本上，一个骨架被放入一个网格中，网格的顶点被绑定到骨架上。这样，当移动骨架时，网格会变形为各种姿势。



As teaching Blender is beyond the scope of this book, the keywords you are looking for are posing and armatures. Armatures are what Blender calls the bones. There are plenty of video tutorials available explaining how to do this. The screenshot below shows the scene with Suzanne and the armatures in Blender. Notice that the ear armatures have been named, so that we can identify them from QML.

由于本书的范围超出了 Blender 的教学，你正在寻找的关键词是姿势和骨架。骨架是 Blender 称之为骨骼的东西。有很多视频教程解释了如何做到这一点。下面的截图显示了 Blender 中的 Suzanne 和骨架的场景。注意，耳朵骨架已经被命名，这样我们就可以从 QML 中识别它们了。

![image](./assets/blender-monkey-with-bones.png)

Once the Blender scene is done, we export it as a COLLADA file and convert it to a QML and a mesh, just as in the _Working with Assets_ section. The resulting QML file is called ``Monkey_with_bones.qml``.

一旦 Blender 场景完成，我们将其导出为 COLLADA 文件，并将其转换为 QML 和网格，就像在 _使用资产_ 部分一样。生成的 QML 文件名为 ``Monkey_with_bones.qml``。

We then have to refer to the files in our ``qt_add_qml_module`` statement in the ``CMakeLists.txt`` file:

然后，我们必须在 ``CMakeLists.txt`` 文件中的 ``qt_add_qml_module`` 语句中引用这些文件：

```
qt_add_qml_module(appanimations
    URI animations
    VERSION 1.0
    QML_FILES main.qml Monkey_with_bones.qml
    RESOURCES meshes/suzanne.mesh
)
```

Exploring the generated QML, we notice that the skeleton is built up from QML elements of the types ``Skeleton`` and ``Joint``. It is possible to work with these elements as code in QML, but it is much more common to create them in a design tool.

查看生成的 QML，我们发现骨架由 ``Skeleton`` 和 ``Joint`` 类型的 QML 元素组成。在 QML 中，可以像代码一样使用这些元素，但更常见的是在设计工具中创建它们。

<<< @/docs/ch12-qtquick3d/src/animations/Monkey_with_bones.qml#armature

The ``Skeleton`` element is then referred to by the ``skeleton`` property of the ``Model`` element, before the ``inverseBindPoses`` property, linking the joints to the model.

然后，在 ``Model`` 元素的 ``inverseBindPoses`` 属性之前，通过 ``skeleton`` 属性引用 ``Skeleton`` 元素，将关节链接到模型上。

<<< @/docs/ch12-qtquick3d/src/animations/Monkey_with_bones.qml#model

The next step is to include the newly created ``Monkey_with_bones`` element into our main ``View3D`` scene:

下一步是将新创建的 ``Monkey_with_bones`` 元素包含到我们的主 ``View3D`` 场景中：


<<< @/docs/ch12-qtquick3d/src/animations/main.qml#monkey

And then we create a ``SequentialAnimation`` built from two ``NumberAnimations`` to make the ear flop forth and back.

然后我们创建一个由两个 ``NumberAnimations`` 构成的 ``SequentialAnimation``，使耳朵向前和向后摆动。



<<< @/docs/ch12-qtquick3d/src/animations/main.qml#animation

::: tip Caveat
In order to be able to access the ``Joint``'s ``eulerRotation.y`` from outside of the ``Monkey_with_bones`` file, we need to expose it as a top level property alias. This means modifying a generated file, which is not very nice, but it solves the problem.

为了能够从 ``Monkey_with_bones`` 文件外部访问 ``Joint`` 的 ``eulerRotation.y``，我们需要将其公开为顶级属性别名。这意味着修改一个生成的文件，这不是很好，但它解决了问题。

<<< @/docs/ch12-qtquick3d/src/animations/Monkey_with_bones.qml#hack
:::

The resulting floppy ear can be enjoyed below:

结果可以享受下面的 floppy 耳朵：

![image](./assets/monkey.gif)

As you can see, it is convenient to import and use skeletons created in a design tool. This makes it convenient to animate complex 3D models from QML.

如你所见，从设计工具中导入和使用骨架是很方便的。这使得从 QML 中对复杂的 3D 模型进行动画处理变得很方便。
