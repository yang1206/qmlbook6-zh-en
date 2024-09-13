# Qt Quick 3D(Qt Quick 3D)

The Qt Quick 3D module takes the power of QML to the third dimension. Using Qt Quick 3D you can create three dimensional scenes and use the property bindings, state management, animations, and more from QML to make the scene interactive. You can even mix 2D and 3D contents in various way to create a mixed environment.

Qt Quick 3D模块将QML的力量提升到了第三个维度。使用Qt Quick 3D，您可以创建三维场景，并使用QML的属性绑定、状态管理、动画等功能使场景具有交互性。您甚至可以通过各种方式混合2D和3D内容，以创建一个混合环境。

Just as Qt provides an abstraction for 2D graphics, Qt Quick 3D relies on an abstraction layer for the various rendering APIs supported. In order to use Qt Quick 3D it is recommended to use a platform provding at least one of the following APIs:

就像Qt为2D图形提供了一个抽象一样，Qt Quick 3D依赖于对支持的各种渲染API的抽象。为了使用Qt Quick 3D，建议使用提供以下API之一的平台：

- OpenGL 3.3+ (support from 3.0)
- OpenGL ES 3.0+ (limited support for OpenGL ES 2)
- Direct3D 11.1
- Vulcan 1.0+
- Metal 1.2+

The Qt Quick Software Adaption, i.e. the software only rendering stack, does not support 3D contents.

即Qt Quick软件适配，即仅软件渲染堆栈，不支持3D内容。


In this chapter we will take you through the basics of Qt Quick 3D, letting you create interactive 3D scenes based on built in meshes as well as assets created in external tools. We will also look at animations and mixing of 2D and 3D contents.

在本章中，我们将带您了解Qt Quick 3D的基础知识，让您基于内置网格以及在外部工具中创建的资产创建交互式3D场景。我们还将介绍动画和2D和3D内容的混合。
<!--
    
## Advanced topics

_on hold_
    
- Custom Materials
    - Shaders 
        - Fragment shader
            - Colour
            - Transparency
            - Texture (images)
        - Vertex shader
            - Basic deformation example
            - Animating the deformation
- Effects
    - Always a fragment shader, applied to the view
    - Play with colour
    - Play with distortion
    - Combine / stack effects
    - Animate effects
- Optimizations
    - Instancing
        - https://doc.qt.io/qt-6/quick3d-instancing.html
    - Improving performance using the shadergen tool
        - https://doc.qt.io/qt-6/qtquick3d-tool-shadergen.html
    - Optimizing models
        - https://doc.qt.io/qt-6/quick3d-asset-conditioning-3d-assets.html
    - Optimizing 2D contents (textures)
        - https://doc.qt.io/qt-6/quick3d-asset-conditioning-2d-assets.html

-->
