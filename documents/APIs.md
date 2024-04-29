[Common API](#common-api)

- [addEventHandler](#addeventhandler)
- [setup](#setup)
- [getRegistrationID](#getregistrationid)
- [stopPush](#stoppush)
- [setChannelAndSound](#setChannelAndSound)
- [resumePush](#resumepush)
- [setAlias](#setalias)
- [getAlias](#getAlias)
- [deleteAlias](#deletealias)
- [addTags](#addtags)
- [deleteTags](#deletetags)
- [setTags](#settags)
- [cleanTags](#cleantags)
- [getAllTags](getalltags)
- [sendLocalNotification](#sendlocalnotification)
- [clearAllNotifications](#clearallnotifications)


[iOS Only]()

- [applyPushAuthority](#applypushauthority)
- [setBadge](#setbadge)
- [getLaunchAppNotification](#getlaunchappnotification)
- [pageEnterTo](#pageEnterTo)
- [pageLeave](#pageLeave)

**注意：addEventHandler 方法建议放到 setup 之前，其他方法需要在 setup 方法之后调用，**

####  addEventHandler

添加事件监听方法。

```dart
JPush jpush = new JPush();
jpush.addEventHandler(
      // 接收通知回调方法。
      onReceiveNotification: (Map<String, dynamic> message) async {
         print("flutter onReceiveNotification: $message");
      },
      // 点击通知回调方法。
      onOpenNotification: (Map<String, dynamic> message) async {
        print("flutter onOpenNotification: $message");
      },
      // 接收自定义消息回调方法。
      onReceiveMessage: (Map<String, dynamic> message) async {
        print("flutter onReceiveMessage: $message");
      },
      onConnected: (Map<String, dynamic> message) async {
        print("flutter onConnected: $message");
      },
  );
```

#### setup

添加初始化方法，调用 setup 方法会执行两个操作：

- 初始化 JPush SDK
- 将缓存的事件下发到 dart 环境中。

**注意：** 插件版本 >= 0.0.8 android 端支持在 setup 方法中动态设置 channel，动态设置的 channel 优先级比 manifestPlaceholders 中的 JPUSH_CHANNEL 优先级要高。

```dart
JPush jpush = new JPush();
jpush.setup(
      appKey: "替换成你自己的 appKey",
      channel: "theChannel",
      production: false,
      debug: false, // 设置是否打印 debug 日志
    );
```

* iOS使用推送功能，需要调用申请通知权限的方法，详情见 [applyPushAuthority](#applyPushAuthority)。

```
jpush.applyPushAuthority(new NotificationSettingsIOS(
      sound: true,
      alert: true,
      badge: true));
```

#### getRegistrationID

获取 registrationId，这个 JPush 运行通过 registrationId 来进行推送.

```dart
JPush jpush = new JPush();
jpush.getRegistrationID().then((rid) { });
```

#### stopPush

停止推送功能，调用该方法将不会接收到通知。

```dart
JPush jpush = new JPush();
jpush.stopPush();
```


####  setChannelAndSound
动态配置 channel 、channel id 及sound，优先级比 AndroidManifest 里配置的高
备注：channel、channel id为必须参数，否则接口调用失败。sound 可选

#### 参数说明
- Object

|参数名称|参数类型|参数说明|
|:-----:|:----:|:-----:|
|channel|string|希望配置的 channel|
|channel_id|string|希望配置的 channel id|
|sound|string|希望配置的 sound(铃声名称，只有铃声名称即可，如AA.mp3,填写AA )|

#### 示例
```dart
JPush jpush = new JPush();
jpush.setChannelAndSound();
```


#### resumePush

调用 stopPush 后，可以通过 resumePush 方法恢复推送。

```dart
JPush jpush = new JPush();
jpush.resumePush();
```

#### setAlias

设置别名，极光后台可以通过别名来推送，一个 App 应用只有一个别名，一般用来存储用户 id。

```
JPush jpush = new JPush();
jpush.setAlias("your alias").then((map) { });
```

#### deleteAlias

可以通过 deleteAlias 方法来删除已经设置的 alias。

```dart
JPush jpush = new JPush();
jpush.deleteAlias().then((map) {})
```
#### getAlias

获取当前 alias 列表。

```
JPush jpush = new JPush();
jpush.getAlias().then((map) { });
```
#### addTags

在原来的 Tags 列表上添加指定 tags。

```
JPush jpush = new JPush();
jpush.addTags(["tag1","tag2"]).then((map) {});
```

####  deleteTags

在原来的 Tags 列表上删除指定 tags。

```
JPush jpush = new JPush();
jpush.deleteTags(["tag1","tag2"]).then((map) {});
```

#### setTags

重置 tags。

```dart
JPush jpush = new JPush();
jpush.setTags(["tag1","tag2"]).then((map) {});
```

#### cleanTags

清空所有 tags

```dart
jpush.setTags().then((map) {});
```

#### getAllTags

获取当前 tags 列表。

```dart
JPush jpush = new JPush();
jpush.getAllTags().then((map) {});
```

#### sendLocalNotification

指定触发时间，添加本地推送通知。

```dart
// 延时 3 秒后触发本地通知。
JPush jpush = new JPush();
var fireDate = DateTime.fromMillisecondsSinceEpoch(DateTime.now().millisecondsSinceEpoch + 3000);
var localNotification = LocalNotification(
   id: 234,
   title: 'notification title',
   buildId: 1,
   content: 'notification content',
   fireTime: fireDate,
   subtitle: 'notification subtitle', // 该参数只有在 iOS 有效
   badge: 5, // 该参数只有在 iOS 有效
   extras: {"fa": "0"} // 设置 extras ，extras 需要是 Map<String, String>
  );
jpush.sendLocalNotification(localNotification).then((res) {});
```

#### clearAllNotifications

清楚通知栏上所有通知。

```dart
JPush jpush = new JPush();
jpush.clearAllNotifications();
```

#### applyPushAuthority

申请推送权限，注意这个方法只会向用户弹出一次推送权限请求（如果用户不同意，之后只能用户到设置页面里面勾选相应权限），需要开发者选择合适的时机调用。

**注意： iOS10+ 可以通过该方法来设置推送是否前台展示，是否触发声音，是否设置应用角标 badge**

```dart
JPush jpush = new JPush();
jpush.applyPushAuthority(new NotificationSettingsIOS(
      sound: true,
      alert: true,
      badge: true));
```

**iOS Only**

#### setBadge

设置应用 badge 值，该方法还会同步 JPush 服务器的的 badge 值，JPush 服务器的 badge 值用于推送 badge 自动 +1 时会用到。

```dart
JPush jpush = new JPush();
jpush.setBadge(66).then((map) {});
```

### getLaunchAppNotification

获取 iOS 点击推送启动应用的那条通知。

```dart
JPush jpush = new JPush();
jpush.getLaunchAppNotification().then((map) {});
```

### pageEnterTo

进入页面，应用内消息功能需要配置该接口，请与pageLeave函数配套使用

```dart
JPush jpush = new JPush();
jpush.pageEnterTo("页面名");
```

### pageLeave

离开页面，应用内消息功能需要配置该接口，请与pageEnterTo函数配套使用

```dart
JPush jpush = new JPush();
jpush.pageLeave("页面名");
```

