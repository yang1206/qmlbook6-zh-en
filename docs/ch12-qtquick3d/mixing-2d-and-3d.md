# Mixing 2D and 3D Contents（混合2D和3D内容）

Qt Quick 3D has been built to integrate nicely into the traditional Qt Quick used to build dynamic 2D contents.

Qt Quick 3D已经构建，可以很好地集成到用于构建动态2D内容的传统Qt Quick中。

## 3D Contents in a 2D Scene（2D场景中的3D内容）

It is straight forward to mix 3D contents into a 2D scene, as the ``View3D`` element represents a 2D surface in the Qt Quick scene.

将3D内容混合到2D场景中很简单，因为``View3D``元素代表Qt Quick场景中的2D表面。



There are a couple of properties that can be of interest when combining 3D contents into a 2D scene this way. 

以这种方式将3D内容混合到2D场景中时，有几个属性可能会感兴趣。

First, the ``renderMode`` of ``View3D``, which lets you control if the 3D contents is rendered behind, in-front-of, or inline with the 2D contents. It can also be rendered on an off-screen buffer which then is combined with the 2D scene.

首先，``View3D``的``renderMode``，它允许您控制3D内容是在2D内容之后、之前还是内联渲染。它还可以渲染在离屏缓冲区上，然后与2D场景结合。


The other property is the ``backgroundMode`` of the ``SceneEnvironment`` bound to the ``environment`` property of the ``View3D``. Ẁe've seen it set to ``Color`` or ``SkyBox``, but it can also be set to ``Transparent`` which lets you see any 2D contents behind the ``View3D`` through the 3D scene.

另一个属性是绑定到``View3D``的``environment``属性的``SceneEnvironment``的``backgroundMode``。我们已经看到它被设置为``Color``或``SkyBox``，但它也可以设置为``Transparent``，这样您就可以通过3D场景看到``View3D``后面的任何2D内容。


When building a combined 2D and 3D scene, it is also good to know that it is possible to combine multiple ``View3D`` elements in a single Qt Quick scene. For instance, if you want to have multiple 3D models in a single scene, but they are separate parts of the 2D interface, then you can put them in separate ``View3D`` elements and handle the layout from the 2D Qt Quick side.

构建一个混合了2D和3D的场景时，还需要知道，可以在单个Qt Quick场景中组合多个``View3D``元素。例如，如果您想在单个场景中拥有多个3D模型，但它们是2D界面的单独部分，那么您可以将它们放在单独的``View3D``元素中，并通过2D Qt Quick侧处理布局。

## 2D Contents in a 3D Scene(3D场景中的2D内容）)

To put 2D contents into a 3D scene, it needs to be placed on a 3D surface. The Qt Quick 3D ``Texture`` element has a ``sourceItem`` property that allows you to integrate a 2D Qt Quick scene as a texture for an arbitrary 3D surface.

要将2D内容放入3D场景中，需要将其放置在3D表面上。Qt Quick 3D的``Texture``元素有一个``sourceItem``属性，允许您将2D Qt Quick场景作为任意3D表面的纹理集成。

Another apporoach is to put the 2D Qt Quick elements directly into the scene. This is the approach used in the example below where we provide a name badge for Suzanne.

另一种方法是将2D Qt Quick元素直接放入场景中。在下面的示例中，我们为Suzanne提供了一个徽章，使用了这种方法。


![image](./assets/mix-2d-and-3d.png)

What we do here is that we instantiate a ``Node`` that serves as an anchor point in the 3D scene. We then place a ``Rectangle`` and a ``Text`` element inside the ``Node``. These two are 2D Qt Quick elements. We can then control the 3D position, rotation, and scale through the corresponding properties of the ``Node`` element.

我们在这里所做的是实例化一个``Node``，它在3D场景中作为一个锚点。然后我们在``Node``中放置一个``Rectangle``和一个``Text``元素。这两个都是2D Qt Quick元素。然后我们可以通过``Node``元素的相应属性来控制3D位置、旋转和缩放。

<<< @/docs/ch12-qtquick3d/src/mix2d3d/main.qml#2dnode
