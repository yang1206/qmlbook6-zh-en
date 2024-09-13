# Sound Effects（声音效果）

When playing sound effects, the response time from requesting playback until actually playing becomes important. In this situation, the `SoundEffect` element comes in handy. By setting up the `source` property, a simple call to the `play` function immediately starts playback.

当播放声音效果时，从请求播放到实际播放的响应时间变得很重要。在这种情况下，`SoundEffect` 元素就派上用场了。通过设置 `source` 属性，只需调用 `play` 函数即可立即开始播放。

This can be utilized for audio feedback when tapping the screen, as shown below.

如下所示，这可以用于点击屏幕时的音频反馈。

<<< @/docs/ch11-multimedia/src/sound-effects/basic.qml#global

The element can also be utilized to accompany a transition with audio. To trigger playback from a transition, the `ScriptAction` element is used.

该元素还可以用于伴随音频的过渡。要从过渡中触发播放，使用 `ScriptAction` 元素。

The following example shows how sound effects elements can be used to accompany transition between visual states using animations:

以下示例显示了如何使用声音效果元素伴随视觉状态的过渡动画：


<<< @/docs/ch11-multimedia/src/sound-effects/complete.qml#global

In this example, we want to apply a 180 rotation animation to our `Rectangle` whenever the "Flip!" button is clicked. We also want to play a different sound when the rectangle flips in one direction or the other.

在这个例子中，我们希望在点击“翻转！”按钮时对 `Rectangle` 应用 180 度旋转动画。我们还希望在矩形向一个方向或另一个方向翻转时播放不同的声音。

To do so, we first start by loading our effects: 

为了做到这一点，我们首先开始加载我们的效果：

<<< @/docs/ch11-multimedia/src/sound-effects/complete.qml#effects

Then we define two states for our rectangle, `DEFAULT` and `REVERSE`, specifying the expected rotation angle for each state: 

然后，我们为矩形定义两个状态，`DEFAULT` 和 `REVERSE`，为每个状态指定预期的旋转角度：

<<< @/docs/ch11-multimedia/src/sound-effects/complete.qml#states

To provide between-states animation, we define two transitions:

为了提供状态之间的动画，我们定义了两个过渡：


<<< @/docs/ch11-multimedia/src/sound-effects/complete.qml#transitions

Notice the `ScriptAction { script: swosh.play(); }` line. Using the `ScriptAction` component we can run an arbitrary script as part of the animation, which allows us to play the desired sound effect as part of the animation.

注意 `ScriptAction { script: swosh.play(); }` 行。使用 `ScriptAction` 组件，我们可以在动画中运行任意脚本，这允许我们在动画中播放所需的声音效果。

::: tip
In addition to the `play` function, a number of properties similar to the ones offered by `MediaPlayer` are available. Examples are `volume` and `loops`. The latter can be set to `SoundEffect.Infinite` for infinite playback. To stop playback, call the `stop` function.

除了 `play` 函数之外，还有一些类似于 `MediaPlayer` 提供的属性。例如，`volume` 和 `loops`。后者可以设置为 `SoundEffect.Infinite` 以进行无限播放。要停止播放，请调用 `stop` 函数。
:::

::: warning
When the PulseAudio backend is used, `stop` will not stop instantaneously, but only prevent further loops. This is due to limitations in the underlying API.

当使用 PulseAudio 后端时，`stop` 不会立即停止，只会阻止进一步的循环。这是由于底层 API 的限制。
:::
:::

