# Materials and Light(材质和光照)

Up until now, we've only worked with basic materials. To create a convincing 3D scene, proper materials and more advanced lighting is needed. Qt Quick 3D supports a number of techniques to achieve this, and in this section we will look at a few of them.

到目前为止，我们只处理了基础材质。要创建一个令人信服的3D场景，需要适当的材质和更高级的光照。Qt Quick 3D 支持多种技术来实现这一目标，在本节中我们将查看其中的一些技术。


## The Built-in Materials(内置材质)

First up, we will look at the built in materials. Qt Quick 3D comes with three material types: ``DefaultMaterial``, ``PrincipledMaterial``, and ``CustomMaterial``. In this chapter we will touch on the two first, while the latter allows you to create truly custom material by providing your own vertex and fragment shaders.

首先，我们将查看内置材质。Qt Quick 3D带有三种材质类型：``DefaultMaterial``、``PrincipledMaterial``和``CustomMaterial``。在本章中，我们将介绍前两种，而后者允许您通过提供自己的顶点和片段着色器来创建真正自定义的材质。


Examples of the two material types can be seen below, with the ``PrincipledMaterial`` to the left, and the ``DefaultMaterial`` to the right.

下面可以看到两种材质类型的示例，``PrincipledMaterial``位于左侧，``DefaultMaterial``位于右侧。



![image](./assets/materials.png)

Comparing the two Suzannes, we can see how the two materials are set up.

比较这两种，我们可以看到这两种材料是如何建立的。


For the ``DefaultMaterial``, we use the ``diffuseColor``, ``specularTint``, and ``specularAmount`` properties. We will look at how variations of these properties affect the appearance of the objects later in this section.

对于``DefaultMaterial``，我们使用``diffuseColor``、``specularTin``t和``specularAmount``属性。在本节后面，我们将研究这些属性的变化如何影响对象的外观。

<<< @/docs/ch12-qtquick3d/src/basicmaterials/main.qml#suzannedefault

For the ``PrincipledMaterial``, we tune the ``baseColor``, ``metalness``, and ``roughness``properties. Again, we will look at how variations of these properties affect the appearance later in this section.

对于``PrincipledMaterial``，我们调整``baseColor``、``metalness``和``roughness``属性。同样，我们将在本节后面研究这些属性的变化如何影响外观。

<<< @/docs/ch12-qtquick3d/src/basicmaterials/main.qml#suzanneprincipled

### Default Material Properties(默认材质属性)

The figure below shows the default material with various values for the ``specularAmount`` and the ``specularRoughness`` properties.

下面显示了默认材质的``specularAmount``和``specularRoughness``属性的各种值。

![image](./assets/default-material.png)

The ``specularAmount`` varies from ``0.8`` (left-most), through ``0.5`` (center), to ``0.2`` (right-most).

``specularAmount``从``0.8``（最左边）变化到``0.5``（中间）再到``0.2``（最右边）。

The ``specularRoughness`` varies from ``0.0`` (top), through ``0.4`` (middle), to ``0.8`` (bottom).

``specularRoughness``从``0.0``（顶部）变化到``0.4``（中间）再到``0.8``（底部）。


The code for the middle ``Model`` is shown below.

中间``Model``的代码如下。



<<< @/docs/ch12-qtquick3d/src/defaultmaterial/main.qml#middle

### Principled Material Properties(基本材质属性)

The figure below shows the principled material with various values for the ``metalness`` and ``roughness``properties.

下面显示了基本材质的``metalness``和``roughness``属性的各种值。

![image](./assets/principled-material.png)

The ``metalness`` varies from ``0.8`` (left-most), through ``0.5`` (center), to ``0.2`` (right-most).

``metalness``从``0.8``（最左边）变化到``0.5``（中间）再到``0.2``（最右边）。

The ``roughness`` varies from ``0.9`` (top), through ``0.6`` (middle), to ``0.3`` (bottom).

``roughness``从``0.9``（顶部）变化到``0.6``（中间）再到``0.3``（底部）。


<<< @/docs/ch12-qtquick3d/src/principledmaterial/main.qml#middle

## Image-based Lighting(基于图像的光照)

One final detail in the main example in this section is the skybox. For this example, we are using an image as skybox, instead of a single colour background.

本节主示例中的最后一个细节是天空盒。对于这个示例，我们使用图像作为天空盒，而不是单一颜色的背景。

![image](./assets/materials.png)

To provide a skybox, assign a ``Texture``to the ``lightProbe`` property of the ``SceneEnvironment`` as shown in the code below. This means that the scene receives image-based light, i.e. that the skybox is used to light the scene. We also adjust the ``probeExposure`` which is used to control how much light is exposed through the probe, i.e. how brightly the scene will be lit. In this scene, we combine the light probe with a ``DirectionalLight`` for the final lighting.

要提供天空盒，将``Texture``分配给``SceneEnvironment``的``lightProbe``属性，如下面的代码所示。这意味着场景接收基于图像的光，即天空盒用于照亮场景。我们还可以调整``probeExposure``，该属性用于控制通过探针暴露的光的量，即场景将有多亮。在这个场景中，我们将光探针与``DirectionalLight``结合使用以获得最终光照。

<<< @/docs/ch12-qtquick3d/src/basicmaterials/main.qml#skybox

In addition to what we show, the orientation of the light probe can be adjusted using the ``probeOrientation`` vector, and the ``probeHorizon`` property can be used to darken the bottom half of the environment, simulating that the light comes from above, i.e. from the sky, rather than from all around.


除了我们展示的内容外，还可以使用``probeOrientation``向量调整光探针的方向，并使用``probeHorizon``属性使环境的下半部分变暗，模拟光来自上方，即来自天空，而不是来自四面八方。