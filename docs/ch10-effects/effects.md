# Effects in QML

In this chapter, we will look at the tools for various effects in QML. The focus will be on:

在本章中，我们将研究QML中各种效果的工具。重点将是：

* Particle Effects（粒子效果）
* Shader Effects（着色器效果）

## Particle Effects（粒子效果）

Particle effects lets us create groups of particles, i.e. instances of a given element. These are generated in a stochastic way and let us work with groups of items rather than individual items. This can be used to create things such as falling leafs, explosions, fire, clouds, and starfields.

粒子效果让我们能够创建粒子群组，即给定元素的实例。这些是以随机的方式生成的，使我们能够处理项目群组而不是单个项目。这可以用来创建诸如落叶、爆炸、火焰、云彩和星空等效果。


## Shader Effects（着色器效果）

Shader effects are applied in the graphics rendering pipeline and allows us to change both the size and colour of any visible QML element. This can be used to create transitions such as the genie effect, waves and curtains, or filters such as blur, grayscale, and blending.

着色器效果应用于图形渲染管道中，并允许我们更改任何可见QML元素的大小和颜色。这可以用来创建诸如 genie 效果、波浪和窗帘等过渡效果，或者模糊、灰度和混合等过滤器。

Shaders are written in a shader language which is then baked and imported into the QML scene, much as other resources. These shaders can then be applied to images or other elements to create advanced visual effects.

着色器是用着色语言编写的，然后被烘焙并导入到 QML 场景中，就像其他资源一样。然后可以将这些着色器应用于图像或其他元素，以创建高级视觉效果。

::: tip
Working with shader effects is an advanced topic.

使用着色器效果是一个高级主题。
:::
