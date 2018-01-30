---
title: 小视频SDK集成指南
categories:  SDK
description: 你对本页的描述
---

![][1]

#### IMSDK集成文档

##### 版本要求

Android支持4.1版本及以上

##### 开发环境

本SDK开发环境为Android Studio|buildToolsVersion 25|JDK 1.8。

##### SDK资源介绍

- SDK包含jar包文件：

  android-xml-json.jar

  eventbus-2.4.0.jar

  simplexml.jar

  apisdk_0923.jar

  ez-vcard-0.9.6.jar

  pinyin4j-2.5.0.jar

  locSDK_6.13.jar

  librcspublic.jar

  ormlite-android-4.49-SNAPSHOT.jar

  ormlite-core-4.49-SNAPSHOT.jar

  httpcore-4.3.2.jar

  httpmime-4.3.5.jar

  baidumapapi_base_v3_7_3.jar

  baidumapapi_map_v3_7_3.jar

  baidumapapi_search_v3_7_3.jar

  baidumapapi_util_v3_7_3.jar

  aspectjrt-1.8.2.jar

  isoparser-1.0.6.jar

  sqlcipher.jar

  libumcs.jar


- SDK包含so文件：

  libBaiduMapSDK_base_v3_7_3.so

  libBaiduMapSDK_map_v3_7_3.so

  libBaiduMapSDK_search_v3_7_3.so

  libBaiduMapSDK_util_v3_7_3.so

  libbambuser.so

  libfetion-amr.so

  libumcs.so

  libv6sdk.so


- SDK包含aar文件：

  androidbase.aar

  imlib.aar

  sdkui.aar

##### 使用说明

- 导入SDK

- 使用时将jar文件及aar文件放入项目中libs文件夹即可，so文件放在libs文件夹中的armeabi文件夹中。

- 接入

- 使用aar：aar 文件放入引用Module的libs目录下，gradle配置文件中把libs 目录放入依赖：

- ```lua
  repositories{
        flatDir{
        dirs 'libs'
        }
  }
  ```

- 在gradle文件中使用依赖的方式引用aar：

- ```lua
  compile(name:'xxx',ext:'aar')
  ```

- 添加配置

- 设置AndroidManifest.xml文件，声明使用权限(必须)

- ```lua
  <!-- 完全的互联网访问权限 -->
  <uses-permission android:name="android.permission.INTERNET" />
  <!-- 查看 Wi-Fi 状态 -->
  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
  <!-- 查看网络状态 -->
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <!-- 读取手机状态和身份 -->
  <uses-permission android:name="android.permission.READ_PHONE_STATE" />
  <!-- 拍照 -->
  <uses-permission android:name="android.permission.CAMERA" />
  <!-- 录音 -->
  <uses-permission android:name="android.permission.RECORD_AUDIO" />
  <!-- 控制振动器 -->
  <uses-permission android:name="android.permission.VIBRATE" />
  <!-- 防止设备休眠 -->
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <!-- 修改/删除 SD 卡中的内容 -->
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
  <!-- 读取主屏幕的设置和快捷方式 -->
  <uses-permission android:name="com.android.launcher.permission.READ_SETTINGS" />
  <uses-permission android:name="com.android.launcher.permission.WRITE_SETTINGS" />

  <!-- 开机时自动启动，Android4.0及以上版本必须加上该权限 -->
  <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
  <!-- 修改全局系统设置，Settings.System.putInt()时使用 -->
  <uses-permission android:name="android.permission.WRITE_SETTINGS" />
  <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
  <uses-permission android:name="android.permission.BROADCAST_STICKY" />
  <uses-permission android:name="android.permission.READ_LOGS" />
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
  <uses-permission android:name="android.permission.GET_TASKS" />

  <!--for Samsung-->
  <uses-permission android:name="com.sec.android.provider.badge.permission.READ" />
  <uses-permission android:name="com.sec.android.provider.badge.permission.WRITE" />

  <!--for htc-->
  <uses-permission android:name="com.htc.launcher.permission.READ_SETTINGS" />
  <uses-permission android:name="com.htc.launcher.permission.UPDATE_SHORTCUT" />

  <!--for sony-->
  <uses-permission android:name="com.sonyericsson.home.permission.BROADCAST_BADGE" />
  <uses-permission android:name="com.sonymobile.home.permission.PROVIDER_INSERT_BADGE" />

  <!--for apex-->
  <uses-permission android:name="com.anddoes.launcher.permission.UPDATE_COUNT" />

  <!--for solid-->
  <uses-permission android:name="com.majeur.launcher.permission.UPDATE_BADGE" />

  <!--for huawei-->
  <uses-permission android:name="com.huawei.android.launcher.permission.CHANGE_BADGE" />
  <uses-permission android:name="com.huawei.android.launcher.permission.READ_SETTINGS" />
  <uses-permission android:name="com.huawei.android.launcher.permission.WRITE_SETTINGS" />
  <!-- 允许应用改变网络的连接状态 -->
  <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
  <!-- 允许装备或解除可移除的存储仓库的文件系统 -->
  <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
  <!-- 完全的互联网访问权限 -->
  <uses-permission android:name="android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS" />
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  ```

- 同时在gradle中配置如下依赖：

- ```lua
  compile 'com.android.support:support-v4:23.1.1'
  compile 'com.google.code.gson:gson:2.7'
  compile 'com.jude:swipebackhelper:3.1.2'
  compile('com.facebook.fresco:fresco:0.12.0') {
          exclude module: 'support-v4'
  }
  compile('com.facebook.fresco:animated-gif:0.12.0') {
          exclude module: 'support-v4'
  }
  ```


- SDK初始化及方法使用

  1. SDK初始化：

     在主项目的Application类中调用ImSDK.createInstace(Application appCtx, String appName)方法进行SDK的初始化操作。其中方法第二个参数为集成SDK应用的app名。

     SDK在IMSDK类中提供setSDKInitListener(SdkInitListener listener)方法来监听SDK是否初始化成功。

  2. 创建BroadcastReceiver类接收SDK登录相关action：

     |              Action               |                    说明                    |
     | :-------------------------------: | :--------------------------------------: |
     | ImLib.BROADCAST_ACTION_NEED_LOGIN |            需要登录时会发送该action的广播            |
     |   ImLib.BROADCAST_ACTION_LOGIN    | 根据SDK登录的不同状态会根据该action将状态给到主应用，通过ImLib.EXTRA_LOGIN_STATUS获取RFLoginStatus登录状态信息，包含Success、Logining、Failed、kickoff。 |

  3. 登录操作：

     当主项目登录成功获取AuthCode后调用ImSDK.getInstance().login(mobileNumber,authCode)方法进行IM的登录操作。

  4. 好友数据：

     SDK提供好友及用户的数据信息类com.feinno.rongfly.core.modules.contact.model.UserInfo。

     UserInfo字段解释：

     |    字段    |        说明         |
     | :------: | :---------------: |
     |  number  |      用户的手机号码      |
     |   name   |       用户姓名        |
     | portrait |     用户头像网络路径      |
     |  pinyin  |      用户姓名拼音       |
     | isFriend | 该用户与当前登录用户是否为好友关系 |

     主项目可通过SDK提供的ImSDK.getInstance().addUserInfoData(String mobile, UserInfo info)方法，将用户数据给到SDK缓存集合中。

  5. IMSDK类：

     | IMSDK中提供的相关方法                            | 说明                            |
     | ---------------------------------------- | ----------------------------- |
     | setUserInfoCbk(ImLib.IUserInfoCallback cbk） | 当SDK需要获取某个用户信息时会调用该回调         |
     | setUserInfoListCbk(ImLib.IUserInfoListCallback callback) | 当SDK需要获取全部好友列表时会调用            |
     | setGroupMemberInfoCbk(ImLib.IGroupMemberInfoCallBack callBack) | 当SDK获取群组中某些用户信息时调用，手机号之间以\|相隔 |
     | setStartActivityToFriendCbk(IStartActivityToFriend cbk) | 聊天列表页面跳转主项目好友列表时触发            |
     | startActivityToDetailCbk(IStartActivityToDetail detailCallBack) | 当SDK点击用户头像时跳转详情页面会调用          |
     | addHeader(Activity activity, View view)  | 给聊天列表页面添加HeaderView           |
     | removeHeader(Activity activity, View view) | 移除聊天列表页面HeaderView            |
     | setLinkMessageClick(ImLib.ISharedLinkClickCallBack callBack) | 设置分享链接消息体点击事件                 |
     | openSharedActivity(Context context, SharedInfo info) | SDK提供打开分享链接转发页面               |
     | setLogStatus(boolean b)                  | 设置SDK是否打印Log开关，false为关闭Log    |
     | becomeBuddy(String mobile)               | SDK提供添加好友后在                   |
     | scanQRCode(Activity activity, String resultString) | SDK提供扫码入群方法                   |
     | openGroupNoticeAct(Activity activity)    | 开启入群审核列表页面                    |
     | getUnReadCount(TextView view)            | 获取当前SDK未读消息数量                 |

  6. 聊天列表页面：

     SDK提供聊天列表ChatFragment，可直接放在Activity加载展示。刷新未读消息数量，需要先调用EventBus.getDefault().register(this)方法注册后在类中添加如下代码：

     ```lua
     public void onEventMainThread(EBInbox event) {
         LogFeinno.d("msg","接到通知"+event);
         switch (event.getEvent()) {
             case Update:
                 //更新未读的总条数
                 ImSDK.getInstance().getUnReadCount(mTextViewUnReadCount);
                 break;
             default:
                 break;

         }
     }
     ```

  7. 分享链接

     SDK提供分享链接数据类com.feinno.rongfly.core.modules.session.model.SharedInfo。

     SharedInfo字段解释：

     | 字段          | 说明          |
     | ----------- | ----------- |
     | title       | 分享消息体展示标题   |
     | description | 分享链接消息描述    |
     | thumbLink   | 分享链接的展示图片地址 |
     | bodyLink    | 分享链接的具体链接地址 |

     ​

  8. 退出登录

     当主项目用户退出登录时调用ImSDK.getInstance().logout()方法，可以使SDK进行登出操作。

  ​

[1]: http://static.zybuluo.com/darren6/5xquvbieqa2lb8co4bdr6jhd/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20171113132910.png