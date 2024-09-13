# Summary(总结)

Qt Quick 3D offers a rich way of integrating 3D contents into a Qt Quick scene, allowing a tight integration through QML. 

Qt Quick 3D提供了一种将3D内容集成到Qt Quick场景的丰富方法，允许通过QML进行紧密集成。

When working with 3D contents, the most common approach is to work with assets created in other tools such as Blender, Maya, or 3ds Max. Using the _Balsam_ tool it is possible to import meshes, materials, as well as animation skeletons, from these models into QML. This can then be used to render, as well as interacting with the models.

当处理3D内容时，最常见的方法是使用Blender、Maya或3ds Max等工具创建的资产。使用_Balsam_工具，可以将这些模型中的网格、材质以及动画骨架导入到QML中。然后可以使用这些内容进行渲染以及与模型进行交互。


QML is still used to setup the scene, as well as instantiating models. This means that a scene can be built in an external tool, or be instantiated dynamically from QML using elements created using external tool. In the most basic cases, scenes can also be created from the built in meshed that come with Qt Quick 3D.

QML仍然用于设置场景以及实例化模型。这意味着场景可以在外部工具中构建，也可以使用外部工具创建的元素从QML中动态实例化。在最简单的情况下，场景也可以使用Qt Quick 3D附带的内置网格创建。



By allowing the tight integration of Qt Quick's 2D contents, and Qt Quick 3D, it is possible to create modern and intuit user interfaces. With QML's ability to bind C++ properties to QML properties, this makes it easy to connect 3D model state to underlying C++ state.

通过允许Qt Quick的2D内容和Qt Quick 3D的紧密集成，可以创建现代且直观的用户界面。通过QML将C++属性绑定到QML属性的能力，这使得将3D模型状态连接到底层的C++状态变得容易。

In this chapter we've only scratched the surface of what is possible using Qt Quick 3D. There are more advanced concepts ranging from custom filters and shaders, to generating meshes dynamically from C++. There is also a large set of optimization techniques that can be used to ensure good rendering performance of complex 3D contents. You can read more about this in the [Qt Quick 3D Reference Documentation](https://doc.qt.io/qt-6/qtquick3d-index.html).

在本章中，我们只是触及了使用Qt Quick 3D可以实现的内容的表面。还有更多高级概念，包括自定义滤镜和着色器，以及从C++动态生成网格。还有一组可以用来确保复杂3D内容良好渲染性能的优化技术。你可以在[Qt Quick 3D参考文档](https://doc.qt.io/qt-6/qtquick3d-index.html)中了解更多信息。
