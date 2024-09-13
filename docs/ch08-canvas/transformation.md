# Transformation（变换）

The canvas allows you to transform the coordinate system in several ways. This is very similar to the transformation offered by QML items. You have the possibility to `scale`, `rotate`, `translate` the coordinate system. Indifference to QML the transform origin is always the canvas origin. For example to scale a path around its center you would need to translate the canvas origin to the center of the path. It is also possible to apply a more complex transformation using the transform method.

画布允许你以多种方式变换坐标系。这与QML项提供的变换非常相似。你可以对坐标系进行 `scale`（缩放）、`rotate`（旋转）、`translate`（平移）。不同于QML的是，变换原点始终是画布的原点。例如，要围绕一个路径的中心进行缩放，你需要将画布的原点平移到路径的中心。也可以使用变换方法应用更复杂的变换。

<<< @/docs/ch08-canvas/src/canvas/transform.qml#M1

![image](./assets/transform.png)

Besides translate the canvas allows also to scale using `scale(x,y)` around x and y-axis, to rotate using `rotate(angle)`, where the angle is given in radius (*360 degree = 2\*Math.PI*) and to use a matrix transformation using the `setTransform(m11, m12, m21, m22, dx, dy)`.

除了平移，画布还允许使用 `scale(x,y)` 在x轴和y轴上缩放，使用 `rotate(angle)` 旋转，其中角度以弧度表示（*360度 = 2\*Math.PI*），以及使用 `setTransform(m11, m12, m21, m22, dx, dy)` 使用矩阵变换。

::: tip
To reset any transformation you can call the `resetTransform()` function to set the transformation matrix back to the identity matrix:

要重置任何变换，可以调用 `resetTransform()` 函数将变换矩阵重置为单位矩阵：

```js
ctx.resetTransform()
```
:::
