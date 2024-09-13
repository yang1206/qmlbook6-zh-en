# Video Streams（视频流）

The `VideoOutput` element is not limited to be used in combination with a `MediaPlayer` element. It can also be used with various video sources to display video streams. 

`VideoOutput`元素不仅限于与`MediaPlayer`元素组合使用。它还可以与各种视频源组合使用以显示视频流。


For instance, we can use the `VideoOutput` to display the live video stream of the user's `Camera`. To do so, we will combine it with two components: `Camera` and `CaptureSession`.

例如，我们可以使用`VideoOutput`来显示用户的`Camera`的实时视频流。为此，我们将它与两个组件：`Camera`和`CaptureSession`组合在一起。

<<< @/docs/ch11-multimedia/src/camera-capture/basic.qml#global

The `CaptureSession` component provides a simple way to read a camera stream, capture still images or record videos.

`CaptureSession`组件提供了一种简单的方法来读取相机流，捕获静态图像或录制视频。


As the `MediaPlayer` component, the `CaptureSession` element provides a `videoOuput` attribute. We can thus use this attribute to configure our own visual component.

与`MediaPlayer`组件一样，`CaptureSession`元素提供了一个`videoOuput`属性。因此，我们可以使用此属性来配置自己的视觉组件。


Finally, when the application is loaded, we can start the camera recording:

最后，当应用程序加载时，我们可以开始相机录制：

```qml
Component.onCompleted: captureSession.camera.start()
```

::: tip
Depending on your operating system, this application may require sensitive access permission(s). If you run this sample application using the `qml` binary, those permissions will be requested automatically.

根据您的操作系统，此应用程序可能需要敏感访问权限。如果您使用`qml`二进制文件运行此示例应用程序，则会自动请求这些权限。

However, if you run it as an independant program you may need to request those permissions first (e.g.: under MacOS, you would need a dedicated .plist file bundled with your application).

但是，如果您作为独立程序运行它，您可能需要先请求这些权限（例如：在MacOS上，您需要与您的应用程序捆绑的专用.plist文件）。

:::
