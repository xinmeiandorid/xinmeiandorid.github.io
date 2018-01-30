### iOS_IMSDK集成指南

2017年9月

#### 修订历史

描述：A-增加 M-修改 D-删除

| 版本   | 时间        | 修订人  | 描述   |
| ---- | --------- | ---- | ---- |
| V1.0 | 2017/9/19 | 焦瑞雪  | 新建文档 |
| V2.0 |           |      |      |
| V3.0 |           |      |      |
| V4.0 |           |      |      |
| V5.0 |           |      |      |
| V6.0 |           |      |      |
| V7.0 |           |      |      |
| V8.0 |           |      |      |
| V9.0 |           |      |      |

#### 1      简介

RongflyIMKit（融飞即时通讯能力库）目前开放了注册登录、二人消息、群组模块，消息类型包括文字、表情、图片、语音、小视频、红包、地理位置等。通过RongflyIMKit，开发者可以快速将即时通讯能力根据自己的业务需要灵活集成到APP中。

RongflyIMKit目前提供的是含UI的接入方式，暂时不支持深度定制，但是可以提供消息类型、背景色、icon图片等方面的定制。

##### 1.1      获取SDK

通过SDK业务人员获取SDK。

##### 1.2      获取AppKey

通过SDK业务人员获取AppKey。

##### 1.3      获取AppSecret

通过SDK业务人员获取AppSecret。 

#### 2       SDK集成

##### 2.1     导入SDK

###### 2.1.1  SDK文件说明

SDK包含以下文件：

| **文件**                 | **说明**                 | **注意事项**           |
| ---------------------- | ---------------------- | ------------------ |
| RongflyIMKit.framework | IMKit 的  framework 库   | 必须导入               |
| RongflyIMKit.bundle    | 资源包，包含图片、storyboard等资源 | 必须导入，否则界面将无法显示     |
| Images.xcassets        | 图片包                    | 必须导入，否则界面某些图片将无法显示 |
| zh-Hans.lproj          | 中文语言包                  | 必须导入，否则界面将无法显示中文   |
| en.lproj               | 英文语言包                  | 必须导入，否则界面有些英文将无法显示 |

另外，还有以下第三方类库：

| **文件**          | **说明**  | **注意事项**        |
| --------------- | ------- | --------------- |
| BaiduMap        | 百度地图SDK | 可选，如目标工程包含则不需导入 |
| libopencore-amr | 音频录制类库  | 可选，如目标工程包含则不需导入 |
| libtriesearch   | 搜索类库    | 可选，如目标工程包含则不需导入 |
| libzbar         | 二维码类库   | 可选，如目标工程包含则不需导入 |
| openssl-1.0.2d  | 加密类库    | 可选，如目标工程包含则不需导入 |
| SDWebImage      | 图片加载框架  | 可选，如目标工程包含则不需导入 |
| SSZipArchive    | 文件加解压类库 | 可选，如目标工程包含则不需导入 |
| YYKit           | 组件框架    | 可选，如目标工程包含则不需导入 |

###### 2.1.2  添加系统库依赖

需要添加以下系统库依赖：

- CoreGraphics.framework
- CoreLocation.framework
- CoreTelephony.framework
- OpenGLES.framework
- QuartzCore.framework
- SystemConfiguration.framework
- libc++.tbd
- libiconv.2.4.0.tbd
- libsqlite3.0.tbd
- libstdc++.6.0.9.tbd
- libxml2.2.tbd

###### 2.1.3  编译设置

在Build Settings中需要增加以下设置：

-   Other Linker Flags增加“-Objc”、“-licucore”。
-   Enable Bitcode设置为NO。

###### 2.1.4  属性列表

在Info.plist中需要增加以下设置：

- 增加LSApplicationQueriesSchemes，允许打开高德地图、百度地图。
- 增加Privacy - Camera Usage Description，允许使用相机。
- 增加Privacy - Photo Library UsageDescription，允许使用相册。
- 增加Privacy - Location When In UseUsage Description、Privacy -Location Usage Description，允许使用定位。
- 增加App Transport Security Settings，设置Allow Arbitrary Loads为YES，支持HTTPS请求。
- 增加View controller-based status barappearance，设置为NO。

##### 2.2     使用SDK

###### 2.2.1  头文件

在需要使用SDK功能的类中，import相关头文件：

- RongflyIMKit.h，包含了所有API，请仔细阅读。

- DistributeConfig.h，用于控制SDK的日志开关。

###### 2.2.2  初始化

使用SDK提供的功能之前，必须先使用AppKey初始化SDK，API如下：

```
/**
 使用AppKey初始化融飞SDK
 注：在使用融飞SDK的功能前，必须先调用该方法进行初始化

 @param appKey 平台分配给应用的AppKey
 */
- (void)initWithAppKey:(nonnull NSString *)appKey;
```

###### 2.2.3  登录

登录前，首先需要从融飞服务器获取authCode，用于身份认证，相关业务对接请咨询后台开发。

然后调用登录API：

```
/**
 登录融飞SDK
 
 @param mobileNo 登录的手机号码
 @param authCode AppServer从融飞服务器获取的鉴权码
 */
- (void)loginWithMobileNo:(nonnull NSString *)mobileNo
                 authCode:(nonnull NSString *)authCode;
```

###### 2.2.4  用户信息

融飞服务器和SDK不存储APP的用户信息，需要使用用户信息的时候通过RongFlyUserInfoDelegate获取， APP需要实现RongFlyUserInfoDelegate的协议。
