# Multimedia（多媒体）

The multimedia elements in the Qt Multimedia makes it possible to playback and record media such as sound, video or pictures. Decoding and encoding are handled through platform-specific backends. For instance, the popular GStreamer framework is used on Linux, WMF is used on Windows, AVFramework on OS X and iOS and the Android multimedia APIs are used on Android.

Qt 多媒体模块使得播放和录制如声音、视频或图片等多媒体内容成为可能。编解码工作是通过特定于平台的后端来处理的。例如，在 Linux 上使用流行的 GStreamer 框架，在 Windows 上使用 WMF，在 OS X 和 iOS 上使用 AVFoundation，在 Android 上则使用 Android 的多媒体 API。

The multimedia elements are not a part of the Qt Quick core API. Instead, they are provided through a separate API made available by importing Qt Multimedia as shown below:

多媒体元素不是Qt Quick核心API的一部分。相反，它们是通过导入Qt多媒体提供的单独API提供的，如下所示：

```qml
import QtMultimedia
```

