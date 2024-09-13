# Playing Media(播放媒体)

The most basic case of multimedia integration in a QML application is for it to playback media. The `QtMultimedia` module supports this by providing a dedicated QML component: the `MediaPlayer`.


在 QML 应用程序中最基本的多媒体集成情况是播放媒体。`QtMultimedia` 模块通过提供一个专门的 QML 组件 `MediaPlayer` 来支持这一点。

The `MediaPlayer` component is a non-visual item that connects a media source to one or several output channel(s). Depending on the nature of the media (i.e. audio, image or video) various output channel(s) can be configured.

`MediaPlayer` 组件是一个非可视项，它将媒体源连接到一个或多个输出通道。根据媒体的本质（即音频、图像或视频），可以配置各种输出通道。

## Playing audio

In the following example, the `MediaPlayer` plays a mp3 sample audio file from a remote URL in an empty window:

在下面的示例中，`MediaPlayer` 在一个空窗口中播放来自远程 URL 的 mp3 示例音频文件：

<<< @/docs/ch11-multimedia/src/playback-audio/main.qml#global

In this example, the `MediaPlayer` defines two attributes: 

在这个例子中，`MediaPlayer` 定义了两个属性：

- `source`: it contains the URL of the media to play. It can either be embedded (`qrc://`), local (`file://`) or remote (`https://`).
- `source`: 它包含要播放的媒体的 URL。它可以是嵌入的（`qrc://`）、本地的（`file://`）或远程的（`https://`）。
- `audioOutput`: it contains an audio output channel, `AudioOutput`, connected to a physical output device. By default, it will use the default audio output device of the system.
- `audioOutput`: 它包含一个连接到物理输出设备的音频输出通道，`AudioOutput`。默认情况下，它将使用系统的默认音频输出设备。

As soon as the main component has been fully initialized, the player’s `play` function is called:

一旦主组件完全初始化，就会调用播放器的 `play` 函数：


<<< @/docs/ch11-multimedia/src/playback-audio/main.qml#play


## Playing a video(播放视频)

If you want to play visual media such as pictures or videos, you must also define a `VideoOutput` element to place the resulting image or video in the user interface.

如果你想要播放视觉媒体，如图片或视频，你还需要定义一个 `VideoOutput` 元素，将生成的图像或视频放置在用户界面中。

In the following example, the `MediaPlayer` plays a mp4 sample video file from a remote URL and centers the video content in the window:

在下面的示例中，`MediaPlayer` 从远程 URL 播放一个 mp4 示例视频文件，并将视频内容居中显示在窗口中：

<<< @/docs/ch11-multimedia/src/playback-video/main.qml#global

In this example, the `MediaPlayer` defines a third attribute:

在这个例子中，`MediaPlayer` 定义了第三个属性：

- `videoOutput`: it contains the video output channel, `VideoOutput`, representing the visual space reserved to display the video in the user interface.
- `videoOutput`: 它包含视频输出通道，`VideoOutput`，表示保留在用户界面中显示视频的视觉空间。

::: tip
Please note that the `VideoOutput` component is a visual item. As such, it's essential that it is created within the visual components hierarchy and not within the `MediaPlayer` itself.

请注意，`VideoOutput` 组件是一个可视项。因此，它必须在可视组件层次结构中创建，而不是在 `MediaPlayer` 本身中创建。
:::


## Controlling the playback(控制播放)

The `MediaPlayer` component offers several useful properties. For instance, the `duration` and `position` properties can be used to build a progress bar. If the `seekable` property is `true`, it is even possible to update the `position` when the progress bar is tapped.

`MediaPlayer` 组件提供了几个有用的属性。例如，`duration` 和 `position` 属性可以用来构建一个进度条。如果 `seekable` 属性为 `true`，当进度条被点击时，还可以更新 `position`。


It's also possible to leverage `AudioOutput` and `VideoOutput` properties to customize the experience and provide, for instance, volume control.

还可以利用 `AudioOutput` 和 `VideoOutput` 属性来定制体验并提供，例如音量控制。


The following example adds custom controls for the playback:

下面的示例为播放添加了自定义控件：


* a volume slider(音量滑块)
* a play/pause button(播放/暂停按钮)
* a progress slider(进度滑块)

<<< @/docs/ch11-multimedia/src/playback-controls/main.qml#global

### The volume slider(音量滑块)
A vertical `Slider` component is added on the top right corner of the window, allowing the user to control the volume of the media:

在窗口右上角添加了一个垂直的 `Slider` 组件，允许用户控制媒体的音量：

<<< @/docs/ch11-multimedia/src/playback-controls/main.qml#volume-slider

The volume attribute of the `AudioOutput` is then mapped to the value of the slider:

然后，将 `AudioOutput` 的音量属性映射到滑块的值：


<<< @/docs/ch11-multimedia/src/playback-controls/main.qml#audio-output

### Play / Pause(播放/暂停)

A `Button` component reflects the playback state of the media and allows the user to control this state: 

一个 `Button` 组件反映了媒体的播放状态，并允许用户控制这个状态：

<<< @/docs/ch11-multimedia/src/playback-controls/main.qml#button

Depending on the playback state, a different text will be displayed in the button. When clicked, the corresponding action will be triggered and will either play or pause the media.

根据播放状态，按钮中会显示不同的文本。当点击时，将触发相应的操作并播放或暂停媒体。



::: tip
The possible playback states are listed below: 

可能的播放状态如下：

* `MediaPlayer.PlayingState`: The media is currently playing.(正在播放媒体。)
* `MediaPlayer.PausedState`: Playback of the media has been suspended.(播放媒体已被暂停。)
* `MediaPlayer.StoppedState`: Playback of the media is yet to begin.(播放媒体尚未开始。)
::: 


### Interactive progress slider(交互式进度滑块)

A `Slider` component is added to reflect the current progress of the playback. It also allows the user to control the current position of the playback.

添加了一个 `Slider` 组件来反映播放的当前进度。它还允许用户控制播放的当前位置。

<<< @/docs/ch11-multimedia/src/playback-controls/main.qml#progress-slider{5,6,19-21}

A few things to note on this sample: 
这个示例中需要注意几点：

* This slider will only be enabled when the media is `seekable` (line 5)
* 滑块只有在媒体可查找时才会启用（第5行）
* Its value will be set to the current media progress, i.e. `player.position / player.duration` (line 6)
* 它们的值将被设置为当前的媒体进度，即 `player.position / player.duration`（第6行）
* The media position will be *(also)* updated when the slider is moved by the user (lines 19-21)
* 当滑块被用户移动时，媒体位置将被 *(也)* 更新（第19-21行）

## The media status（媒体状态）

When using `MediaPlayer` to build a media player, it is good to monitor the `status` property of the player. Here is an enumeration of the possible statuses, ranging from `MediaPlayer.Buffered` to `MediaPlayer.InvalidMedia`. The possible values are summarized in the bullets below:

使用 `MediaPlayer` 构建媒体播放器时，监控播放器的 `status` 属性是个好主意。这里有一个可能的状态的枚举，从 `MediaPlayer.Buffered` 到 `MediaPlayer.InvalidMedia`。可能的值总结在下面的子弹中：



* `MediaPlayer.NoMedia`. No media has been set. Playback is stopped.（没有设置媒体。播放已停止。）
* `MediaPlayer.Loading`. The media is currently being loaded.（媒体正在加载中。）
* `MediaPlayer.Loaded`. The media has been loaded. Playback is stopped.（媒体已加载。播放已停止。）
* `MediaPlayer.Buffering`. The media is buffering data.（媒体正在缓冲数据。）
* `MediaPlayer.Stalled`. The playback has been interrupted while the media is buffering data.（播放在媒体缓冲数据时被中断。）
* `MediaPlayer.Buffered`. The media has been buffered, this means that the player can start playing the media.（媒体已被缓冲，这意味着播放器可以开始播放媒体。）
* `MediaPlayer.EndOfMedia`. The end of the media has been reached. Playback is stopped.（媒体已到达末尾。播放已停止。）
* `MediaPlayer.InvalidMedia`. The media cannot be played. Playback is stopped.（媒体无法播放。播放已停止。）
* `MediaPlayer.UnknownStatus`. The status of the media is unknown.（媒体的状态未知。）

As mentioned in the bullets above, the playback state can vary over time. Calling `play`, `pause` or `stop` alters the state, but the media in question can also have an effect. For example, the end can be reached, or it can be invalid, causing playback to stop. 

如上所述，播放状态可能会随时间而变化。调用 `play`、`pause` 或 `stop` 会改变状态，但所讨论的媒体也可能产生影响。例如，可以到达末尾，或者它可能是无效的，导致播放停止。

::: tip
It is also possible to let the `MediaPlayer` to loop a media item. The `loops` property controls how many times the `source` is to be played. Setting the property to `MediaPlayer.Infinite` causes endless looping. Great for continuous animations or a looping background song.

也可以让 `MediaPlayer` 循环播放媒体项。`loops` 属性控制 `source` 要播放多少次。将属性设置为 `MediaPlayer.Infinite` 将导致无限循环。非常适合连续动画或循环背景歌曲。
:::
