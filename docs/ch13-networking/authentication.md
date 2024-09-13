# Authentication using OAuth（使用OAuth进行身份验证）

OAuth is an open protocol to allow secure authorization in a simple and standard method from web, mobile, and desktop applications. OAuth is used to authenticate a client against common web-services such as Google, Facebook, and Twitter.

OAuth是一个开放的协议，允许通过简单的标准方法从web、移动和桌面应用程序进行安全授权。OAuth用于根据常见的web服务（如Google、Facebook和Twitter）对客户端进行身份验证。

::: tip
For a custom web-service you could also use the standard HTTP authentication for example by using the `XMLHttpRequest` username and password in the get method (e.g. `xhr.open(verb, url, true, username, password)`)

对于自定义web服务，您也可以使用标准的HTTP身份验证，例如通过在get方法中使用`XMLHttpRequest`用户名和密码（例如`xhr.open(verb, url, true, username, password)`）
:::

OAuth is currently not part of a QML/JS API. So you would need to write some C++ code and export the authentication to QML/JS. Another issue would be the secure storage of the access token.

OAuth目前不是QML/JS API的一部分。因此，您需要编写一些C++代码，并将身份验证导出到QML/JS。另一个问题是安全存储访问令牌。


Here are some links which we find useful:

这里有一些我们认为有用的链接：

* [http://oauth.net/](http://oauth.net/)
* [http://hueniverse.com/oauth/](http://hueniverse.com/oauth/)
* [https://github.com/pipacs/o2](https://github.com/pipacs/o2)
* [http://www.johanpaul.com/blog/2011/05/oauth2-explained-with-qt-quick/](http://www.johanpaul.com/blog/2011/05/oauth2-explained-with-qt-quick/)

## Integration example（集成示例）

In this section, we will go through an example of OAuth integration using the [Spotify API](https://developer.spotify.com/documentation/web-api/). This example uses a combination of C++ classes and QML/JS. To discover more on this integration, please refer to Chapter 16.

在本节中，我们将通过使用[Spotify API](https://developer.spotify.com/documentation/web-api/)的OAuth集成示例。此示例结合了C++类和QML/JS。要了解有关此集成的更多信息，请参阅第16章。


This application's goal is to retrieve the top ten favourite artists of the authenticated user.

该应用程序的目标是检索经过身份验证用户的十大最爱艺术家。

### Creating the App（创建应用程序）

First, you will need to create a dedicated app on the [Spotify Developer's portal](https://developer.spotify.com/dashboard/applications). 

首先，您需要在[Spotify开发者门户](https://developer.spotify.com/dashboard/applications)上创建一个专用应用程序。

![image](./images/oauth-spotify-dashboard.png)

Once your app is created, you'll receive two keys: a `client id` and a `client secret`. 

创建应用程序后，您将收到两个密钥：`client id`和`client secret`。

![image](./images/oauth-spotify-app.png)

### The QML file（QML文件）

The process is divided in two phases:

该过程分为两个阶段：

1. The application connects to the Spotify API, which in turns requests the user to authorize it;

1. 应用程序连接到Spotify API，然后Spotify API会要求用户授权它；

2. If authorized, the application displays the list of the top ten favourite artists of the user.

2. 如果已授权，应用程序将显示用户的十大最爱艺术家的列表。


#### Authorizing the app（授权应用程序）

Let's start with the first step:

让我们从第一步开始：


<<< @/docs/ch13-networking/src/oauth/main.qml#imports

When the application starts, we will first import a custom library, `Spotify`, that defines a `SpotifyAPI` component (we'll come to that later). This component will then be instantiated:

当应用程序启动时，我们首先导入一个自定义库`Spotify`，该库定义了一个`SpotifyAPI`组件（稍后我们将讨论）。然后，该组件将被实例化：

<<< @/docs/ch13-networking/src/oauth/main.qml#setup

Once the application has been loaded, the `SpotifyAPI` component will request an authorization to Spotify:

一旦应用程序加载完毕，`SpotifyAPI`组件将向Spotify请求授权：


<<< @/docs/ch13-networking/src/oauth/main.qml#on-completed

Until the authorization is provided, a busy indicator will be displayed in the center of the app.

在提供授权之前，应用程序中心将显示一个繁忙指示器。

:::tip
Please note that for security reasons, the API credentials should never be put directly into a QML file!

请注意，出于安全原因，API凭据永远不应该直接放入QML文件中！
:::

#### Listing the user's favorite artists（列出用户的喜爱艺术家）

The next step happens when the authorization has been granted. To display the list of artists, we will use the Model/View/Delegate pattern:

下一步发生在授权被授予之后。要显示艺术家的列表，我们将使用Model/View/Delegate模式：


<<< @/docs/ch13-networking/src/oauth/main.qml#model-view{3,10,13,22,42,51,57,61}

The model `SpotifyModel` is defined in the `Spotify` library. To work properly, it needs a `SpotifyAPI`.

模型`SpotifyModel`在`Spotify`库中定义。为了正常工作，它需要一个`SpotifyAPI`。


The ListView displays a vertical list of artists. An artist is represented by a name, an image and the total count of followers.

ListView显示一个垂直艺术家列表。艺术家由名称、图像和粉丝总数表示。

### SpotifyAPI

Let's now get a bit deeper into the authentication flow. We'll focus on the `SpotifyAPI` class, a `QML_ELEMENT` defined on the C++ side.

现在让我们更深入地了解身份验证流程。我们将关注`SpotifyAPI`类，这是一个在C++端定义的`QML_ELEMENT`。


```cpp
#ifndef SPOTIFYAPI_H
#define SPOTIFYAPI_H

#include <QtCore>
#include <QtNetwork>
#include <QtQml/qqml.h>

#include <QOAuth2AuthorizationCodeFlow>

class SpotifyAPI: public QObject
{
    Q_OBJECT
    QML_ELEMENT

    Q_PROPERTY(bool isAuthenticated READ isAuthenticated WRITE setAuthenticated NOTIFY isAuthenticatedChanged)

public:
    SpotifyAPI(QObject *parent = nullptr);

    void setAuthenticated(bool isAuthenticated) {
        if (m_isAuthenticated != isAuthenticated) {
            m_isAuthenticated = isAuthenticated;
            emit isAuthenticatedChanged();
        }
    }

    bool isAuthenticated() const {
        return m_isAuthenticated;
    }

    QNetworkReply* getTopArtists();

public slots:
    void setCredentials(const QString& clientId, const QString& clientSecret);
    void authorize();

signals:
    void isAuthenticatedChanged();

private:
    QOAuth2AuthorizationCodeFlow m_oauth2;
    bool m_isAuthenticated;
};

#endif // SPOTIFYAPI_H
```

First, we'll import the `<QOAuth2AuthorizationCodeFlow>` class. This class is a part of the `QtNetworkAuth` module, which contains various implementations of `OAuth`.

首先，我们将导入`<QOAuth2AuthorizationCodeFlow>`类。这个类是`QtNetworkAuth`模块的一部分，该模块包含各种`OAuth`的实现。

```cpp
#include <QOAuth2AuthorizationCodeFlow>
```

Our class, `SpotifyAPI`, will define a `isAuthenticated` property:

我们的类`SpotifyAPI`将定义一个`isAuthenticated`属性：

```cpp
void setCredentials(const QString& clientId, const QString& clientSecret);
void authorize();
```

And a private member representing the authentication flow:

表示身份验证流的私有成员：

```cpp
QOAuth2AuthorizationCodeFlow m_oauth2;
```

On the implementation side, we have the following code:

在实现方面，我们有以下代码：

```cpp
#include "spotifyapi.h"

#include <QtGui>
#include <QtCore>
#include <QtNetworkAuth>

SpotifyAPI::SpotifyAPI(QObject *parent): QObject(parent), m_isAuthenticated(false) {
    m_oauth2.setAuthorizationUrl(QUrl("https://accounts.spotify.com/authorize"));
    m_oauth2.setAccessTokenUrl(QUrl("https://accounts.spotify.com/api/token"));
    m_oauth2.setScope("user-top-read");

    m_oauth2.setReplyHandler(new QOAuthHttpServerReplyHandler(8000, this));
    m_oauth2.setModifyParametersFunction([&](QAbstractOAuth::Stage stage, QMultiMap<QString, QVariant> *parameters) {
        if(stage == QAbstractOAuth::Stage::RequestingAuthorization) {
            parameters->insert("duration", "permanent");
        }
    });

    connect(&m_oauth2, &QOAuth2AuthorizationCodeFlow::authorizeWithBrowser, &QDesktopServices::openUrl);
    connect(&m_oauth2, &QOAuth2AuthorizationCodeFlow::statusChanged, [=](QAbstractOAuth::Status status) {
        if (status == QAbstractOAuth::Status::Granted) {
            setAuthenticated(true);
        } else {
            setAuthenticated(false);
        }
    });
}

void SpotifyAPI::setCredentials(const QString& clientId, const QString& clientSecret) {
    m_oauth2.setClientIdentifier(clientId);
    m_oauth2.setClientIdentifierSharedKey(clientSecret);
}

void SpotifyAPI::authorize() {
    m_oauth2.grant();
}

QNetworkReply* SpotifyAPI::getTopArtists() {
    return m_oauth2.get(QUrl("https://api.spotify.com/v1/me/top/artists?limit=10"));
}
```

The constructor task mainly consists in configuring the authentication flow. First, we define the Spotify API routes that will serve as authenticators.

构造函数的任务主要在于配置身份验证流。首先，我们定义了将作为身份验证器的Spotify API路由。


```cpp
m_oauth2.setAuthorizationUrl(QUrl("https://accounts.spotify.com/authorize"));
m_oauth2.setAccessTokenUrl(QUrl("https://accounts.spotify.com/api/token"));
```

We then select the scope (= the Spotify authorizations) that we want to use:

然后，我们选择要使用的范围（=Spotify授权）：


```cpp
m_oauth2.setScope("user-top-read");
````

Since OAuth is a two-way communication process, we instantiate a dedicated local server to handle the replies:

由于OAuth是一个双向通信过程，我们实例化了一个专用本地服务器来处理回复：

```cpp
m_oauth2.setReplyHandler(new QOAuthHttpServerReplyHandler(8000, this));
```

Finally, we configure two signals and slots.

最后，我们配置两个信号和槽。


```cpp
connect(&m_oauth2, &QOAuth2AuthorizationCodeFlow::authorizeWithBrowser, &QDesktopServices::openUrl);
connect(&m_oauth2, &QOAuth2AuthorizationCodeFlow::statusChanged, [=](QAbstractOAuth::Status status) { /* ... */ })
```

The first one configures the authorization to happen within a web-browser (through `&QDesktopServices::openUrl`), while the second makes sure that we are notified when the authorization process has been completed.

第一个配置授权在Web浏览器中发生（通过`&QDesktopServices::openUrl`），而第二个确保我们在授权过程完成后得到通知。

The `authorize()` method is only a placeholder for calling the underlying `grant()` method of the authentication flow. This is the method that triggers the process.

`authorize()`方法只是调用身份验证流底层`grant()`方法的占位符。这是触发过程的方法。

```cpp
void SpotifyAPI::authorize() {
    m_oauth2.grant();
}
```

Finally, the `getTopArtists()` calls the web api using the authorization context provided by the `m_oauth2` network access manager.

最后，`getTopArtists()`使用`m_oauth2`网络访问管理器提供的授权上下文调用Web API。

```cpp
QNetworkReply* SpotifyAPI::getTopArtists() {
    return m_oauth2.get(QUrl("https://api.spotify.com/v1/me/top/artists?limit=10"));
}
```

### The Spotify model

This class is a `QML_ELEMENT` that subclasses `QAbstractListModel` to represent our list of artists. It relies on `SpotifyAPI` to gather the artists from the remote endpoint.

这个类是一个`QML_ELEMENT`，它继承自`QAbstractListModel`来表示我们的艺术家列表。它依赖于`SpotifyAPI`从远程端点获取艺术家。

```cpp
#ifndef SPOTIFYMODEL_H
#define SPOTIFYMODEL_H

#include <QtCore>

#include "spotifyapi.h"

QT_FORWARD_DECLARE_CLASS(QNetworkReply)

class SpotifyModel : public QAbstractListModel
{
    Q_OBJECT
    QML_ELEMENT

    Q_PROPERTY(SpotifyAPI* spotifyApi READ spotifyApi WRITE setSpotifyApi NOTIFY spotifyApiChanged)

public:
    SpotifyModel(QObject *parent = nullptr);

    void setSpotifyApi(SpotifyAPI* spotifyApi) {
        if (m_spotifyApi != spotifyApi) {
            m_spotifyApi = spotifyApi;
            emit spotifyApiChanged();
        }
    }

    SpotifyAPI* spotifyApi() const {
        return m_spotifyApi;
    }

    enum {
        NameRole = Qt::UserRole + 1,
        ImageURLRole,
        FollowersCountRole,
        HrefRole,
    };

    QHash<int, QByteArray> roleNames() const override;

    int rowCount(const QModelIndex &parent) const override;
    int columnCount(const QModelIndex &parent) const override;
    QVariant data(const QModelIndex &index, int role) const override;

signals:
    void spotifyApiChanged();
    void error(const QString &errorString);

public slots:
    void update();

private:
    QPointer<SpotifyAPI> m_spotifyApi;
    QList<QJsonObject> m_artists;
};

#endif // SPOTIFYMODEL_H
```

This class defines a `spotifyApi` property:

这个类定义了一个`spotifyApi`属性：

```cpp
Q_PROPERTY(SpotifyAPI* spotifyApi READ spotifyApi WRITE setSpotifyApi NOTIFY spotifyApiChanged)
```

An enumeration of Roles (as per `QAbstractListModel`):

一个角色枚举（根据`QAbstractListModel`）：


```cpp
enum {
    NameRole = Qt::UserRole + 1,    // The artist's name
    ImageURLRole,                   // The artist's image
    FollowersCountRole,             // The artist's followers count
    HrefRole,                       // The link to the artist's page
};
```

A slot to trigger the refresh of the artists list:

一个触发艺术家列表刷新的插槽：


```cpp
public slots:
    void update();
```

And, of course, the list of artists, represented as JSON objects:

当然，艺术家列表，表示为JSON对象：

```cpp
public slots:
    QList<QJsonObject> m_artists;
```

On the implementation side, we have:

在实现方面，我们有：

```cpp
#include "spotifymodel.h"

#include <QtCore>
#include <QtNetwork>

SpotifyModel::SpotifyModel(QObject *parent): QAbstractListModel(parent) {}

QHash<int, QByteArray> SpotifyModel::roleNames() const {
    static const QHash<int, QByteArray> names {
        { NameRole, "name" },
        { ImageURLRole, "imageURL" },
        { FollowersCountRole, "followersCount" },
        { HrefRole, "href" },
    };
    return names;
}

int SpotifyModel::rowCount(const QModelIndex &parent) const {
    Q_UNUSED(parent);
    return m_artists.size();
}

int SpotifyModel::columnCount(const QModelIndex &parent) const {
    Q_UNUSED(parent);
    return m_artists.size() ? 1 : 0;
}

QVariant SpotifyModel::data(const QModelIndex &index, int role) const {
    Q_UNUSED(role);
    if (!index.isValid())
        return QVariant();

    if (role == Qt::DisplayRole || role == NameRole) {
        return m_artists.at(index.row()).value("name").toString();
    }

    if (role == ImageURLRole) {
        const auto artistObject = m_artists.at(index.row());
        const auto imagesValue = artistObject.value("images");

        Q_ASSERT(imagesValue.isArray());
        const auto imagesArray = imagesValue.toArray();
        if (imagesArray.isEmpty())
            return "";

        const auto imageValue = imagesArray.at(0).toObject();
        return imageValue.value("url").toString();
    }

    if (role == FollowersCountRole) {
        const auto artistObject = m_artists.at(index.row());
        const auto followersValue = artistObject.value("followers").toObject();
        return followersValue.value("total").toInt();
    }

    if (role == HrefRole) {
        return m_artists.at(index.row()).value("href").toString();
    }

    return QVariant();
}

void SpotifyModel::update() {
    if (m_spotifyApi == nullptr) {
        emit error("SpotifyModel::error: SpotifyApi is not set.");
        return;
    }

    auto reply = m_spotifyApi->getTopArtists();

    connect(reply, &QNetworkReply::finished, [=]() {
        reply->deleteLater();
        if (reply->error() != QNetworkReply::NoError) {
            emit error(reply->errorString());
            return;
        }

        const auto json = reply->readAll();
        const auto document = QJsonDocument::fromJson(json);

        Q_ASSERT(document.isObject());
        const auto rootObject = document.object();
        const auto artistsValue = rootObject.value("items");

        Q_ASSERT(artistsValue.isArray());
        const auto artistsArray = artistsValue.toArray();
        if (artistsArray.isEmpty())
            return;

        beginResetModel();
        m_artists.clear();
        for (const auto artistValue : qAsConst(artistsArray)) {
            Q_ASSERT(artistValue.isObject());
            m_artists.append(artistValue.toObject());
        }
        endResetModel();
    });
}
```

The `update()` method calls the `getTopArtists()` method and handle its reply by extracting the individual items from the JSON document and refreshing the list of artists within the model.

这个`update()`方法调用`getTopArtists()`方法，并通过从JSON文档中提取各个项目来刷新模型中的艺术家列表。

```cpp
auto reply = m_spotifyApi->getTopArtists();

connect(reply, &QNetworkReply::finished, [=]() {
    reply->deleteLater();
    if (reply->error() != QNetworkReply::NoError) {
        emit error(reply->errorString());
        return;
    }

    const auto json = reply->readAll();
    const auto document = QJsonDocument::fromJson(json);

    Q_ASSERT(document.isObject());
    const auto rootObject = document.object();
    const auto artistsValue = rootObject.value("items");

    Q_ASSERT(artistsValue.isArray());
    const auto artistsArray = artistsValue.toArray();
    if (artistsArray.isEmpty())
        return;

    beginResetModel();
    m_artists.clear();
    for (const auto artistValue : qAsConst(artistsArray)) {
        Q_ASSERT(artistValue.isObject());
        m_artists.append(artistValue.toObject());
    }
    endResetModel();
});
```

The `data()` method extracts, depending on the requested model role, the relevant attributes of an Artist and returns as a `QVariant`:

`data()`方法根据请求的模型角色提取艺术家的相关属性，并作为`QVariant`返回：


```cpp
    if (role == Qt::DisplayRole || role == NameRole) {
        return m_artists.at(index.row()).value("name").toString();
    }

    if (role == ImageURLRole) {
        const auto artistObject = m_artists.at(index.row());
        const auto imagesValue = artistObject.value("images");

        Q_ASSERT(imagesValue.isArray());
        const auto imagesArray = imagesValue.toArray();
        if (imagesArray.isEmpty())
            return "";

        const auto imageValue = imagesArray.at(0).toObject();
        return imageValue.value("url").toString();
    }

    if (role == FollowersCountRole) {
        const auto artistObject = m_artists.at(index.row());
        const auto followersValue = artistObject.value("followers").toObject();
        return followersValue.value("total").toInt();
    }

    if (role == HrefRole) {
        return m_artists.at(index.row()).value("href").toString();
    }
```

![image](./images/oauth-spotify-result.png)


