项目集成文档
### 1.概述
这是一个提供扫描识别及生成二维码功能的SDK，针对6.0以上系统进行权限适配，集成方便，便于快速开发项目。

### 2.提供方法

##### QrCodeSDK提供方法
| 方法名                    |             含义             |
| ---------------------- | :------------------------: |
| startCaptureActivity()          |        传递Context上下文即可跳转扫描二维码页面        |
| createQrCode()      |        传递需要生成二维码信息及二维码的大小尺寸        |
| setSuccessCallBack()          |        设置监听便于扫码成功后的结果回调       |

### 3.集成指南

##### 1.AndroidManifest.xml

    <!-- 拍照 -->
    <uses-permission android:name="android.permission.CAMERA" />
    <!-- 控制振动器 -->
    <uses-permission android:name="android.permission.VIBRATE" />

    <activity
        android:name="cn.com.fetion.qrcode.zxing.activity.CaptureActivity"
        android:screenOrientation="portrait"
        android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
        android:windowSoftInputMode="stateAlwaysHidden" />

##### 2.在 build.gradle 文件中添加依赖

    dependencies {
        compile fileTree(dir: 'libs', include: ['*.jar'])
        compile 'com.android.support:appcompat-v7:26.+'
    }

##### 3.libs

    在libs文件夹中导入core-3.1.0.jar

##### 4.在Application中初始化

    @Override
    public void onCreate() {
        super.onCreate();
        QrCodeSDK.createInstance(getApplicationContext());
    }

##### 5.Android6.0系统及以上版本系统调用扫描页面时需进行相机权限的判断
