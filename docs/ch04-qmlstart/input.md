# Input Elements(输入元素)

We have already used the `MouseArea` as a mouse input element. Next, we’ll focus on keyboard input. We start off with the text editing elements: `TextInput` and `TextEdit`.

我们已经使用了 `MouseArea` 作为鼠标输入元素。接下来，我们将关注键盘输入。我们从文本编辑元素开始：`TextInput` 和 `TextEdit`。

## TextInput

`TextInput` allows the user to enter a line of text. The element supports input constraints such as `validator`, `inputMask`, and `echoMode`.

`TextInput` 允许用户输入一行文本。该元素支持输入约束，如 `validator`、`inputMask` 和 `echoMode`。

```qml
// textinput.qml

import QtQuick

Rectangle {
    width: 200
    height: 80
    color: "linen"

    TextInput {
        id: input1
        x: 8; y: 8
        width: 96; height: 20
        focus: true
        text: "Text Input 1"
    }

    TextInput {
        id: input2
        x: 8; y: 36
        width: 96; height: 20
        text: "Text Input 2"
    }
}
```

![](./assets/textinput.png)


The user can click inside a `TextInput` to change the focus. To support switching the focus by keyboard, we can use the `KeyNavigation` attached property.

用户可以点击 `TextInput` 内部来改变焦点。为了支持通过键盘切换焦点，我们可以使用 `KeyNavigation` 附加属性。

```qml
// textinput2.qml

import QtQuick

Rectangle {
    width: 200
    height: 80
    color: "linen"

    TextInput {
        id: input1
        x: 8; y: 8
        width: 96; height: 20
        focus: true
        text: "Text Input 1"
        KeyNavigation.tab: input2
    }

    TextInput {
        id: input2
        x: 8; y: 36
        width: 96; height: 20
        text: "Text Input 2"
        KeyNavigation.tab: input1
    }
}
```

The `KeyNavigation` attached property supports a preset of navigation keys where an element id is bound to switch focus on the given key press.

`KeyNavigation` 附加属性支持一组预设的导航键，其中元素 id 绑定到在给定键按下时切换焦点。

A text input element comes with no visual presentation beside a blinking cursor and the entered text. For the user to be able to recognize the element as an input element it needs some visual decoration; for example, a simple rectangle. When placing the `TextInput` inside an element you need make sure you export the major properties you want others to be able to access.

文本输入元素除了闪烁的光标和输入的文本外，没有任何视觉表示。为了让用户能够识别该元素为输入元素，它需要一些视觉装饰；例如，一个简单的矩形。当将 `TextInput` 放置在元素内部时，你需要确保你导出了你希望其他用户能够访问的主要属性。


We move this piece of code into our own component called `TLineEditV1` for reuse.

我们将这段代码移动到我们自己的组件 `TLineEditV1` 中，以便重用。

```qml
// TLineEditV1.qml

import QtQuick

Rectangle {
    width: 96; height: input.height + 8
    color: "lightsteelblue"
    border.color: "gray"

    property alias text: input.text
    property alias input: input

    TextInput {
        id: input
        anchors.fill: parent
        anchors.margins: 4
        focus: true
    }
}
```

::: tip
If you want to export the `TextInput` completely, you can export the element by using `property alias input: input`. The first `input` is the property name, where the 2nd input is the element id.

如果你希望完全导出 `TextInput`，你可以通过使用 `property alias input: input` 来导出元素。第一个 `input` 是属性名称，第二个 `input` 是元素 id。
:::

We then rewrite our `KeyNavigation` example with the new `TLineEditV1` component.

然后我们使用新的 `TLineEditV1` 组件重写 `KeyNavigation` 示例。

```qml
Rectangle {
    ...
    TLineEditV1 {
        id: input1
        ...
    }
    TLineEditV1 {
        id: input2
        ...
    }
}
```

![](./assets/textinput3.png)

Try the tab key for navigation. You will experience the focus does not change to `input2`. The simple use of `focus: true` is not sufficient. The problem is that when the focus was transferred to the `input2` element, the top-level item inside the `TlineEditV1` (our `Rectangle`) received focus, and did not forward the focus to the `TextInput`. To prevent this, QML offers the `FocusScope`.

尝试使用 tab 键进行导航。你会发现在焦点不会切换到 `input2`。简单使用 `focus: true` 是不够的。问题是当焦点转移到 `input2` 元素时，`TlineEditV1`（我们的 `Rectangle`）中的顶级项接收了焦点，并将焦点转发给了 `TextInput`。为了防止这种情况，QML 提供了 `FocusScope`。

## FocusScope

A focus scope declares that the last child element with `focus: true` receives the focus when the focus scope receives the focus. So it forwards the focus to the last focus-requesting child element. We will create a second version of our TLineEdit component called TLineEditV2, using a focus scope as the root element.

`FocusScope` 声明当焦点范围接收焦点时，最后一个具有 `focus: true` 的子元素将接收焦点。因此，它将焦点转发给最后一个请求焦点的子元素。我们将创建一个名为 TLineEditV2 的 TLineEdit 组件的第二个版本，使用焦点范围作为根元素。

```qml
// TLineEditV2.qml

import QtQuick

FocusScope {
    width: 96; height: input.height + 8
    Rectangle {
        anchors.fill: parent
        color: "lightsteelblue"
        border.color: "gray"

    }

    property alias text: input.text
    property alias input: input

    TextInput {
        id: input
        anchors.fill: parent
        anchors.margins: 4
        focus: true
    }
}
```

Our example now looks like this:

我们的示例现在看起来像这样：
```qml
Rectangle {
    ...
    TLineEditV2 {
        id: input1
        ...
    }
    TLineEditV2 {
        id: input2
        ...
    }
}
```

Pressing the tab key now successfully switches the focus between the 2 components and the correct child element inside the component is focused.

现在按下 tab 键可以成功地在两个组件之间切换焦点，并且组件内的正确子元素被聚焦。

## TextEdit

The `TextEdit` is very similar to `TextInput`, and supports a multi-line text edit field. It doesn’t have the text constraint properties, as this depends on querying the content size of the text (`contentHeight`, `contentWidth`). We also create our own component called `TTextEdit` to provide an editing background and use the focus scope for better focus forwarding.

`TextEdit` 与 `TextInput` 非常相似，支持多行文本编辑字段。它没有文本约束属性，因为这是查询文本内容大小（`contentHeight`，`contentWidth`）。我们还创建了一个名为 `TTextEdit` 的组件，以提供编辑背景，并使用焦点范围以更好地转发焦点。

```qml
// TTextEdit.qml

import QtQuick

FocusScope {
    width: 96; height: 96
    Rectangle {
        anchors.fill: parent
        color: "lightsteelblue"
        border.color: "gray"

    }

    property alias text: input.text
    property alias input: input

    TextEdit {
        id: input
        anchors.fill: parent
        anchors.margins: 4
        focus: true
    }
}
```

You can use it like the `TLineEdit` component

你可以像使用 `TLineEdit` 组件一样使用它

```qml
// textedit.qml

import QtQuick

Rectangle {
    width: 136
    height: 120
    color: "linen"

    TTextEdit {
        id: input
        x: 8; y: 8
        width: 120; height: 104
        focus: true
        text: "Text Edit"
    }
}
```

![](./assets/textedit.png)

## Keys Element

The attached property `Keys` allows executing code based on certain key presses. For example, to move and scale a square, we can hook into the up, down, left and right keys to translate the element, and the plus and minus keys to scale the element.


附加属性 `Keys` 允许根据某些按键执行代码。例如，要移动和缩放一个正方形，我们可以挂钩到上、下、左和右键来平移元素，以及加号和减号键来缩放元素。


<<< @/docs/ch04-qmlstart/src/input/KeysExample.qml#global

![](./assets/keys.png)

