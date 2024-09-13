# Capturing Images（捕获图像）

One of the key features of the `Camera` element is that is can be used to take pictures. We will use this in a simple stop-motion application. By building the application, you will learn how to show a viewfinder, switch between cameras, snap photos and keep track of the pictures taken.

`Camera` 元素的一个关键特性是它可以用来拍照。我们将使用它来构建一个简单的定格动画应用程序。通过构建应用程序，你将学习如何显示取景器，在相机之间切换，拍摄照片并跟踪拍摄的照片。

The user interface is shown below. It consists of three major parts. In the background, you will find the viewfinder, to the right, a column of buttons and at the bottom, a list of images taken. The idea is to take a series of photos, then click the `Play Sequence` button. This will play the images back, creating a simple stop-motion film.

用户界面如下所示。它由三个主要部分组成。在背景中，你会找到取景器，在右边，有一列按钮，在底部，有一张图片列表。想法是拍摄一系列照片，然后点击“播放序列”按钮。这将回放图片，创建一个简单的定格动画电影。

![image](./assets/camera-ui.png)

## The viewfinder（取景器）

The viewfinder part of the camera is made using a `VideoOutput` element as video output channel of a `CaptureSession`. The `CaptureSession` in turns uses a `Camera` component to configure the device. This will display a live video stream from the camera.

相机取景器部分使用 `VideoOutput` 元素作为 `CaptureSession` 的视频输出通道。`CaptureSession` 又使用 `Camera` 组件来配置设备。这将显示来自相机的实时视频流。


<<< @/docs/ch11-multimedia/src/camera-capture/complete.qml#viewfinder

::: tip
You can have more control on the camera behaviour by using dedicated `Camera` properties such as `exposureMode`, `whiteBalanceMode` or `zoomFactor`.

你可以通过使用 `Camera` 的专用属性，如 `exposureMode`、`whiteBalanceMode` 或 `zoomFactor`，来更细致地控制相机行为。
:::

## The captured images list（捕获的图片列表）

The list of photos is a `ListView` oriented horizontally that shows images from a `ListModel` called `imagePaths`. In the background, a semi-transparent black `Rectangle` is used.

图片列表是一个水平方向的 `ListView`，显示一个名为 `imagePaths` 的 `ListModel` 中的图片。在背景中，使用了一个半透明的黑色 `Rectangle`。

<<< @/docs/ch11-multimedia/src/camera-capture/complete.qml#model-view

For the shooting of images, the `CaptureSession` element contains a set of sub-elements for various tasks. To capture still pictures, the `CaptureSession.imageCapture` element is used. When you call the `captureToFile` method, a picture is taken and saved in the user's local pictures directory. This results in the `CaptureSession.imageCapture` emitting the `imageSaved` signal.

为了拍摄图片，`CaptureSession` 元素包含一组用于各种任务的子元素。要拍摄静态图片，使用 `CaptureSession.imageCapture` 元素。当你调用 `captureToFile` 方法时，会拍摄并保存一张图片到用户的本地图片目录中。这会导致 `CaptureSession.imageCapture` 发射 `imageSaved` 信号。


<<< @/docs/ch11-multimedia/src/camera-capture/complete.qml#button-shot

In this case, we don’t need to show a preview image, but simply add the resulting image to the `ListView` at the bottom of the screen. Shown in the example below, the path to the saved image is provided as the `path` argument with the signal.

在这种情况下，我们不需要显示预览图片，只需将生成的图片添加到屏幕底部的 `ListView` 中。在下面的示例中，保存的图片的路径作为 `path` 参数提供。


<<< @/docs/ch11-multimedia/src/camera-capture/complete.qml#capture-session{6-8}


:::tip
For showing a preview, connect to the `imageCaptured` signal and use the `preview` signal argument as `source` of an `Image` element. An `id` signal argument is sent along both the `imageCaptured` and `imageSaved`. This value is returned from the `capture` method. Using this, the capture of an image can be traced through the complete cycle. This way, the preview can be used first and then be replaced by the properly saved image. This, however, is nothing that we do in the example.

要显示预览，请连接到 `imageCaptured` 信号，并使用 `preview` 信号参数作为 `Image` 元素的 `source`。一个 `id` 信号参数会同时发送给 `imageCaptured` 和 `imageSaved`。这个值会从 `capture` 方法返回。使用这个值，可以通过完整的循环追踪图片的捕获。然而，在示例中，我们并没有这样做。
:::

## Switching between cameras（在相机之间切换）

If the user has multiple cameras, it can be handy to provide a way of switching between those. It's possible to achieve this by using the `MediaDevices` element in conjunction with a `ListView`. In our case, we'll use a `ComboBox` component:

如果用户有多个相机，提供一种在这些相机之间切换的方式会很方便。通过结合使用 `MediaDevices` 元素和 `ListView`，可以实现这一点。在我们的例子中，我们将使用 `ComboBox` 组件：

<<< @/docs/ch11-multimedia/src/camera-capture/complete.qml#switching-devices{1-3,11}

The `model` property of the `ComboBox` is set to the `videoInputs` property of our `MediaDevices`. This last property contains the list of usable video inputs. We then set the `displayText` of the control to the description of the camera device (`captureSession.camera.cameraDevice.description`).

`ComboBox` 的 `model` 属性被设置为 `MediaDevices` 的 `videoInputs` 属性。这个属性包含可用的视频输入列表。然后，我们将控制器的 `displayText` 设置为相机设备的描述 (`captureSession.camera.cameraDevice.description`)。


Finally, when the user switches the video input, the cameraDevice is updated to reflect that change: `captureSession.camera.cameraDevice = cameraComboBox.currentValue`.

最后，当用户切换视频输入时，`cameraDevice` 会更新以反映这种变化：`captureSession.camera.cameraDevice = cameraComboBox.currentValue`。

## The playback（播放）

The last part of the application is the actual playback. This is driven using a `Timer` element and some JavaScript. The `_imageIndex` variable is used to keep track of the currently shown image. When the last image has been shown, the playback is stopped. In the example, the `root.state` is used to hide parts of the user interface when playing the sequence.

应用程序的最后一部分是实际的播放。这是使用 `Timer` 元素和一点 JavaScript 驱动的。`_imageIndex` 变量用于跟踪当前显示的图片。当显示完最后一张图片时，播放会停止。在示例中，`root.state` 用于在播放序列时隐藏用户界面的部分。

<<< @/docs/ch11-multimedia/src/camera-capture/complete.qml#playback
