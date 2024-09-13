# Layout Items(布局项目)

QML provides a flexible way to layout items using anchors. The concept of anchoring is fundamental to `Item`, and is available to all visual QML elements. Anchors act like a contract and are stronger than competing geometry changes. Anchors are expressions of relativeness; you always need a related element to anchor with.

QML提供了一种灵活的方式来使用锚点布局项目。锚的概念是`Item`的基础，并且对所有可视的QML元素都可用。锚点的作用就像一个合同，比竞争的几何变化更强。锚点是相对性的表达；你总是需要一个相关的元素来锚定。

![](./assets/anchors.png)

An element has 6 major anchor lines (`top`, `bottom`, `left`, `right`, `horizontalCenter`, `verticalCenter`). Additionally, there is the `baseline` anchor for text in `Text` elements. Each anchor line comes with an offset. In the case of the `top`, `bottom`, `left`, and `right` anchors, they are called margins. For `horizontalCenter`, `verticalCenter` and `baseline` they are called offsets.

一个元素有6条主要的锚线（`top`，`bottom`，`left`，`right`，`horizontalCenter`，`verticalCenter`）。另外，`Text`元素中的文本有一个`baseline`锚点。每条锚线都有一个偏移量。对于`top`，`bottom`，`left`和`right`锚点，它们被称为边距。对于`horizontalCenter`，`verticalCenter`和`baseline`，它们被称为偏移量。



![](./assets/anchorgrid.png)


* **(1)** An element fills a parent element.
* **(1)** 一个元素填充一个父元素。

    ```qml
    GreenSquare {
        BlueSquare {
            width: 12
            anchors.fill: parent
            anchors.margins: 8
            text: '(1)'
        }
    }
    ```
    


* **(2)** An element is left aligned to the parent.
* **(2)** 一个元素与父元素左对齐。

    ```qml
    GreenSquare {
        BlueSquare {
            width: 48
            y: 8
            anchors.left: parent.left
            anchors.leftMargin: 8
            text: '(2)'
        }
    }
    ```



* **(3)** An element's left side is aligned to the parent’s right side.
* **(3)** 一个元素的左边与父元素的右边对齐。

    ```qml
    GreenSquare {
        BlueSquare {
            width: 48
            anchors.left: parent.right
            text: '(3)'
        }
    }
    ```



* **(4)** Center-aligned elements. `Blue1` is horizontally centered on the parent. `Blue2` is also horizontally centered, but on `Blue1`, and its top is aligned to the `Blue1` bottom line.
* **(4)** 居中对齐的元素。`Blue1`在父元素上水平居中。`Blue2`也在`Blue1`上水平居中，但其顶部与`Blue1`底部对齐。

    ```qml
    GreenSquare {
        BlueSquare {
            id: blue1
            width: 48; height: 24
            y: 8
            anchors.horizontalCenter: parent.horizontalCenter
        }
        BlueSquare {
            id: blue2
            width: 72; height: 24
            anchors.top: blue1.bottom
            anchors.topMargin: 4
            anchors.horizontalCenter: blue1.horizontalCenter
            text: '(4)'
        }
    }
    ```



* **(5)** An element is centered on a parent element
* **(5)** 一个元素在父元素上居中

    ```qml
    GreenSquare {
        BlueSquare {
            width: 48
            anchors.centerIn: parent
            text: '(5)'
        }
    }
    ```



* **(6)** An element is centered with a left-offset on a parent element using horizontal and vertical center lines
* **(6)** 一个元素在父元素上使用水平和垂直中心线居中，并使用左偏移量

    ```qml
    GreenSquare {
        BlueSquare {
            width: 48
            anchors.horizontalCenter: parent.horizontalCenter
            anchors.horizontalCenterOffset: -12
            anchors.verticalCenter: parent.verticalCenter
            text: '(6)'
        }
    }
    ```

## Hidden Gems(隐藏的宝石)

Our squares have been magically enhanced to enable dragging. Try the example and drag around some squares. You will see that (1) can’t be dragged as it’s anchored on all sides (although you can drag the parent of (1), as it’s not anchored at all). (2) can be vertically dragged, as only the left side is anchored. The same applies to (3). (4) can only be dragged vertically, as both squares are horizontally centered. (5) is centered on the parent, and as such, can’t be dragged. The same applies to (6). Dragging an element means changing its `x,y` position. As anchoring is stronger than setting the `x,y` properties, dragging is restricted by the anchored lines. We will see this effect later when we discuss animations.


我们的方块已神奇地增强了拖动功能。试着在示例中拖动一些方格。你会发现（1）无法拖动，因为它的四边都被锚定了（不过你可以拖动（1）的父元素，因为它根本没有被锚定）。(2)可以垂直拖动，因为只有左侧被锚定。这同样适用于（3）。(4)只能垂直拖动，因为两个方格都水平居中。(5) 在父对象上居中，因此无法拖动。(6)也是如此。拖动元素意味着改变其 `x,y` 位置。由于锚定比设置 `x,y` 属性更强，拖动会受到锚定线的限制。我们将在以后讨论动画时看到这种效果。


