# The Basics（基础）

In this section we will walk through the basics of Qt Quick 3D. This includes working with the built in shapes (meshes), using lights, and transformations in 3D.

在本节中，我们将介绍 Qt Quick 3D 的基础知识。这包括使用内置的形状（网格）、使用灯光和 3D 中的变换。

## A Basic Scene（基本场景）

A 3D scene consists of a few standard elements:

3D 场景由几个标准元素组成：

- ``View3D``, which is the top level QML element representing the entire 3D scene.
- ``View3D``, 一个表示整个 3D 场景的顶层 QML 元素。
- ``SceneEnvironment``, controls how the scene is rendered, including how the background, or sky box, is rendered.
- ``SceneEnvironment``, 控制场景的渲染方式，包括如何渲染背景或天空盒。
- ``PerspectiveCamera``, the camera in the scene. Can also be a ``OrthographicCamera``, or even a custom camera with a custom projection matrix.
- ``PerspectiveCamera``, 场景中的相机。也可以是 ``OrthographicCamera``，甚至是一个具有自定义投影矩阵的自定义相机。

In addition to this, the scene usually contains ``Model`` instances representing objects in the 3D space, and lights.

除了这些，场景通常包含表示 3D 空间中的对象的 ``Model`` 实例和灯光。

We will look at how these elements interact by creating the scene shown below.

我们将通过创建下面的场景来了解这些元素是如何交互的。

![image](./assets/basicscene.png)

First of all, the QML code is setup with a ``View3D`` as the main element, filling the window. We also import the ``QtQuick3D`` module.

首先，QML 代码使用 ``View3D`` 作为主元素，填充窗口。我们还导入了 ``QtQuick3D`` 模块。


The ``View3D`` element can be seen as any other Qt Quick element, just that inside of it, the 3D contents will be rendered.

``View3D`` 元素可以看作是任何其他 Qt Quick 元素，只是其中的 3D 内容将被渲染。

```qml
import QtQuick
import QtQuick3D

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Basic Scene")

    View3D {
        anchors.fill: parent

        // ...
    
    }
}
```

Then we setup the ``SceneEnvironment`` with a solid background colour. This is done inside the ``View3D`` element.

然后我们使用纯色背景设置 ``SceneEnvironment``。这是在 ``View3D`` 元素内部完成的。

<<< @/docs/ch12-qtquick3d/src/basicscene/main.qml#environment

The ``SceneEnvironment`` can be used to control a lot more rendering parameters, but for now, we only use it to set a solid background colour.

``SceneEnvironment`` 可以用来控制很多渲染参数，但现在我们只使用它来设置纯色背景。


The next step is to add _meshes_ to the scene. A mesh represents an object in 3D space. Each mesh is created using a ``Model`` QML element.

下一步是将 _网格_ 添加到场景中。网格表示 3D 空间中的对象。每个网格都是使用 ``Model`` QML 元素创建的。

A model can be used to load 3D assets, but there are a few built-in meshes allowing us to get started without involving the complexity of 3D assets management. In the code below, we create a ``#Cone`` and a ``#Sphere``.

模型可以用来加载 3D 资产，但有一些内置的网格，允许我们开始而不涉及 3D 资产管理的复杂性。在下面的代码中，我们创建了一个 ``#Cone`` 和一个 ``#Sphere``。

In addition to the shape of the mesh, we position them in 3D space and provide them with a material with a simple, diffuse base colour. We will discuss materials more in the [Materials and Light]("Materials and Lights") section

除了网格的形状，我们在 3D 空间中定位它们，并为它们提供一个具有简单漫反射基色的材质。我们将在 [材质和灯光](./materials.md) 部分更详细地讨论材质。

When positioning elements in 3D space, coordinates are expressed as ``Qt.vector3d(x, y, z)`` where the `x` axis controls the horizontal movement, `y` is the vertical movement, and `z` the how close or far away something is. 

在 3D 空间中定位元素时，坐标表示为 ``Qt.vector3d(x, y, z)``，其中 `x` 轴控制水平移动，`y` 是垂直移动，`z` 控制某物是靠近还是远离。

here the rotation is done around the `x`, `y`, and `z` axes in that order. The rotation is expressed in degrees.

在这里，旋转是按照 `x`、`y` 和 `z` 轴的顺序进行的。旋转以度为单位表示。


By default, the positive direction of the `x` axis is to the right, positive `y` points upwards, and positive `z` out of the screen. I say default, because this depends on the projection matrix of the camera.

默认情况下，`x` 轴的正方向向右，正 `y` 指向上方，正 `z` 指向屏幕外。我说默认，因为这取决于相机的投影矩阵。


<<< @/docs/ch12-qtquick3d/src/basicscene/main.qml#meshes

Once we have lights in the scene we add a ``DirectionalLight``, which is a light that works much like the sun. It adds an even light in a pre-determined direction. The direction is controlled using the ``eulerRotation`` property where we can rotate the light direction around the various axes.

一旦我们在场景中添加了灯光，我们添加一个 ``DirectionalLight``，它的工作方式很像太阳。它在预定的方向上添加均匀的光。方向使用 ``eulerRotation`` 属性控制，我们可以围绕各种轴旋转光的方向。

By setting the ``castsShadow`` property to ``true`` we ensure that the light generates shadows as can be seen on cone, where the shadow from the sphere is visible.

通过将 ``castsShadow`` 属性设置为 ``true``，我们确保光线产生阴影，如圆锥体所示，其中球体的阴影可见。

<<< @/docs/ch12-qtquick3d/src/basicscene/main.qml#light

The last piece of the puzzle is to add a camera to the scene. There are various cameras for various perspectives, but for a realistic projection, the ``ProjectionCamera`` is the one to use.

最后一个难题是将相机添加到场景中。有各种相机用于各种视角，但对于逼真的投影，``ProjectionCamera`` 是要使用的相机。


In the code, we place the camera using the ``position`` property. It is then possible to direct the camera using the ``eulerRotation`` property, but instead we call the ``lookAt`` method from the ``Component.onCompleted`` signal handler. This rotates the camera to look at a specific direction once it has been created and initialized.

在代码中，我们使用 ``position`` 属性放置相机。然后可以使用 ``eulerRotation`` 属性来指导相机，但相反，我们从 ``Component.onCompleted`` 信号处理程序中调用 ``lookAt`` 方法。一旦相机被创建和初始化，它就会旋转以朝一个特定的方向看。

<<< @/docs/ch12-qtquick3d/src/basicscene/main.qml#camera

The resulting scene can be seen below.

结果场景如下所示。

![image](./assets/basicscene.png)

So, all in all, a minimal scene consists of a ``View3D`` with an ``SceneEnvironment``, something to look at, e.g. a ``Model`` with a mesh, a light source, e.g. a ``DirectionalLight``, and something to look with, e.g. a ``PerspectiveCamera``.


总的来说，一个最小的场景由一个 ``View3D``，一个 ``SceneEnvironment``，一些东西来观看，例如一个带有网格的 ``Model``，一个光源，例如一个 ``DirectionalLight``，以及一些东西来观看，例如一个 ``PerspectiveCamera``。


## The Built-in Meshes（内置网格）

In the previous example, we used the built-in cone and sphere. Qt Quick 3D comes with the following built in meshes:

在前面的示例中，我们使用了内置的圆锥体和球体。Qt Quick 3D 附带了以下内置网格：

- ``#Cube``
- ``#Cone``
- ``#Sphere``
- ``#Cylinder``
- ``#Rectangle``

These are all shown in the illustration below. (top-left: Cube, top-right: Cone, center: Sphere, bottom-left: Cylinder, bottom-right: Rectangle)

这些都可以在下面的图中看到。（左上角：立方体，右上角：圆锥体，中间：球体，左下角：圆柱体，右下角：矩形）

![image](./assets/meshes.png)

::: tip Tip
One caveat is that the ``#Rectangle`` is one-sided. That means that it is only visible from one direction. This means that the ``eulerRotation`` property is important.

一个警告是 ``#Rectangle`` 是单面的。这意味着它只能从一侧看到。这意味着 ``eulerRotation`` 属性很重要。
:::

When working with real scenes, the meshes are exported from a design tool and then imported into the Qt Quick 3D scene. We look at this in more detail in the [Working with Assets](assets.md) section.

当我们处理真实场景时，网格是从设计工具中导出的，然后导入到 Qt Quick 3D 场景中。我们将在 [使用资产](assets.md) 部分中更详细地讨论这个问题。



## Lights（灯光）

Just as with meshes, Qt Quick 3D comes with a number of pre-defined light sources. These are used to light the scene in different ways.

正如网格一样，Qt Quick 3D 附带了多种预定义的光源。这些用于以不同的方式照亮场景。

The first one, ``DirectionalLight``, should be familiar from our previous example. It works much as the sun, and casts light uniformly over the scene in a given direction. If the ``castsShadow`` property is set to ``true``, the light will cast shadows, as shown in the illustration below. This property is available for all the light sources.

第一个，``DirectionalLight``，应该从我们之前的示例中熟悉。它的工作方式就像太阳一样，并在给定的方向上均匀地照亮场景。如果将 ``castsShadow`` 属性设置为 ``true``，光线将投射阴影，如下图所示。该属性对所有光源都可用。


![image](./assets/light_directional.png)

<<< @/docs/ch12-qtquick3d/src/lights/main.qml#directional

The next light source is the ``PointLight``. It is a light that eminates from a given point in space and then falls off towards darkness based on the values of the ``constantFade``, ``linearFade``, and ``quadraticFace`` properties, where the light is calculated as ``constantFade + distance * (linearFade * 0.01) + distance^2 * (quadraticFade * 0.0001)``. The default values are ``1.0`` constant and quadratic fade, and ``0.0`` for the linear fade, meaning that the light falls off according to the inverse square law.

下一个光源是 ``PointLight``。这是一种从空间中的给定点发出光线的光源，然后根据 ``constantFade``、``linearFade`` 和 ``quadraticFace`` 属性的值逐渐变暗，其中光线的计算方式为 ``constantFade + distance * (linearFade * 0.01) + distance^2 * (quadraticFade * 0.0001)``。默认值为 ``1.0`` 常量和二次衰减，以及 ``0.0`` 线性衰减，这意味着光线根据平方反比定律衰减。

![image](./assets/light_point.png)

<<< @/docs/ch12-qtquick3d/src/lights/main.qml#point

The last of the light sources is the ``SpotLight`` which emits a cone of light in a given direction, much like a real world spotlight. The cone consists of an inner and an outer cone. The width of these is controlled by the ``innerConeAngle`` and ``coneAngle``, specified in degrees between zero and 180 degrees.

最后一个光源是 ``SpotLight``，它以给定的方向发出光锥，就像现实世界中的聚光灯一样。这个锥体由一个内锥体和一个外锥体组成。这些的宽度由 ``innerConeAngle`` 和 ``coneAngle`` 控制，以度为单位，在零到180度之间。

The light in the inner cone behaves much like a ``PointLight`` and can be controlled using the ``constantFade``, ``linearFade``, and ``quadraticFace`` properties. In addition to this, the light fades towards darkness as it approaches the outer cone, controlled by the ``coneAngle``.

内锥体的光线与 ``PointLight`` 类似，可以使用 ``constantFade``、``linearFade`` 和 ``quadraticFace`` 属性进行控制。此外，随着它接近外锥体，光线逐渐变暗，由 ``coneAngle`` 控制。

![image](./assets/light_spot.png)

<<< @/docs/ch12-qtquick3d/src/lights/main.qml#spot

In addition to the ``castsShadow`` property, all lights also has the commonly used properties ``color`` and ``brightness`` which control the color and intensity of the light emitted. The lights also has an ``ambientColor`` property defining a base color to be applied to materials, before they are lit by the light source. This property is set to black by default, but can be used to provide a base lighting in a scene.

除了 ``castsShadow`` 属性外，所有光源还具有常用的 ``color`` 和 ``brightness`` 属性，用于控制发出的光线的颜色和强度。光源还具有一个 ``ambientColor`` 属性，用于定义在光源照亮材质之前要应用于材质的基本颜色。默认情况下，此属性设置为黑色，但可以用于在场景中提供基本照明。

In the examples this far, we've only used one light source at a time, but it is of course possible to combine multiple light sources in a single scene.

到目前为止，我们只使用了一个光源，但当然可以在单个场景中组合多个光源。