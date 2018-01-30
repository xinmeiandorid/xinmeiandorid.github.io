---
title: 小视频SDK集成指南
categories:  SDK
description: 你对本页的描述
---

![][1]

##### 版本要求

Android支持4.0及以上。

##### 开发环境配置

本SDK开发环境为Android Studio|buildToolsVersion 25|JDK 1.8。

##### SDK资源介绍

SDK包括videosdk-1.0.aar文件。

##### 使用说明

- ###### 导入SDK

  使用时只需要将.aar文件放入项目libs文件夹中即可。


- ###### 接入

  使用aar：aar 文件放入引用Module的libs目录下，gradle配置文件中把libs 目录放入依赖：

  ```lua
  repositories{
        flatDir{
        dirs 'libs'
        }
  }
  ```

  在gradle文件中使用依赖的方式引用aar：

  ```lua
  compile(name:'xxx',ext:'aar')
  ```

- ###### 添加配置

  设置AndroidManifest.xml文件，声明使用权限(必须)

  ```lua
  <!-- 拍照 -->
  <uses-permission android:name="android.permission.CAMERA" />
  <!-- 录音 -->
  <uses-permission android:name="android.permission.RECORD_AUDIO" />
  <!-- 防止设备休眠 -->
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <!-- 修改/删除 SD 卡中的内容 -->
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
  ```

  在gradle文件中使用依赖的方式引用v7包

  ```lua
  compile 'com.android.support:appcompat-v7:25.2.0'
  ```

  v7版本根据项目中的buildToolsVersion做出对应的调整，比如本SDK用的是buildToolsVersion "25.0.3"。


- ###### 录制小视频

  小视频的录制主要是基于fragment实现，需要在项目录制小视频的入口处添加对fragment的引用。录制主要分为以下步骤进行操作：

  1. 录制视频界面为Fragment,因此当前Acitivity 需要继承FragmentActivity（必须）；

  2. 首先在需要展示的Activity的Layout中添加一下布局，用于加载录制小视频Fragment的容器；

     ```lua
     <LinearLayout
         	android:id="@+id/layout_record_video_view"
         	android:layout_width="match_parent"
         	android:layout_height="match_parent"
         	android:background="#000000"
         	android:clickable="true"
         	android:orientation="vertical"
         	android:visibility="gone"
     />
     ```

  3. 在Activity中找到上述View

     ```lua
     View mRecordVideoView = findViewById(R.id.layout_record_video_view);
     ```

  4. 打开录制小视频的界面

     ```lua
     rivate String[] permissions = {
             Manifest.permission.READ_EXTERNAL_STORAGE,
             Manifest.permission.WRITE_EXTERNAL_STORAGE,
             Manifest.permission.CAMERA,
             Manifest.permission.RECORD_AUDIO
     };
     //首先判断权限
     ArrayList<String> list = checker.lacksPermissions(permissions);
     boolean isLack = list.size() > 0 ? true : false;
     if (isLack) {
         checker.requestPermission(MainActivity.this, permissions, 3);
     } else {
         mRecordVideoView.setVisibility(View.VISIBLE);
     //参数FragmentActivity 为当前activity;参数containerViewId为第二步添加Fragment的容器View
         VideoManager.getInstance().addRecordeVideoFragment(FragmentActivity.this, R.id.layout_record_video_view);
     }
     ```

  5. 重写权限回调

     ```lua
     @Override
     public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
         super.onRequestPermissionsResult(requestCode, permissions, grantResults);
         if (requestCode == 3 && checker.hasAllPermissionsGranted(grantResults)) {
            	mRecordVideoView.setVisibility(View.VISIBLE);
     		//参数FragmentActivity 为当前activity;参数containerViewId为第二步添加Fragment的容			器View
         	VideoManager.getInstance().addRecordeVideoFragment(MainActivity.this, 				R.id.layout_record_video_view);
         }
     }
     ```

  6. 小视频录制完成回调

     ```lua
      VideoManager.getInstance().setVideoRecordListener(new IVideoRecordListener() {
         @Override
         public void onVideoRecordSuccess(Attachment attachment) {
             mRecordVideoView.setVisibility(View.GONE);
             
         }

         @Override
         public void onVideoRecordFailed() {
             mRecordVideoView.setVisibility(View.GONE);
         }
     });
     ```

Attachment 字段解释：

| 字段             | 说明        |
| -------------- | --------- |
| videoPath      | 视频存储地址    |
| videoThumbPath | 视频缩略图存储地址 |
| width          | 视频宽       |
| height         | 视频高       |
| duration       | 视频时长      |
| bitrate        | 视频比特率     |
| size           | 视频大小      |

  1. 在当前Activity重写OnKeyDown()方法，在录制视频点击返回键时，回到当前Activity

     ```lua
     @Override
     public boolean onKeyDown(int keyCode, KeyEvent event) {
         if (keyCode == KeyEvent.KEYCODE_BACK) {
             if (VideoManager.getInstance().isVideoRecording()) {
                 //如果正在录制视频，删除fragment
                 mRecordVideoView.setVisibility(View.GONE);
                 VideoManager.getInstance().removeRecordeVideoFragment(MainActivity.this);
                 return true;
             }
         }
         return super.onKeyDown(keyCode, event);
     }

     ```


- 播放小视频

  1. 在小窗口中播放录制好的小视频，比如在列表中播放，需要在适配器布局中引用SmartVideoView。	

     ```lua
     <cn.com.fetion.video.play.SmartVideoView
         	android:id="@+id/view_smart_video_play"
         	android:layout_width="192dp"
         	android:layout_height="144dp"
         	android:visibility="visible" />
     ```

  2. SmartVideoView的使用

     ```lua
     SmartVideoView mVideo = (SmartVideoView) view.findViewById(R.id.view_smart_video_play);
     mVideo.setPath(videoPath);
     mVideo.setVisibility(View.VISIBLE);
     mVideo.startPlay(true, new SmartVideoView.SmartVideoPlayListener() {
         @Override
         public boolean onPlayStart() {
     		//开始播放
             return true;
         }

         @Override
         public void onPlayReStart() {
     		//准备播放
         }

         @Override
         public void onPlayError(Exception e) {
     		//播放失败
         }

         @Override
         public void onPlayFinish() {
     		//播放完成
         }
     });
     ```

##### 数据比对

|  名称  |   大小    | 录制时长(s) |  宽度  |  高度  | 数据速率  | 总比特率  | 帧速率(帧/s) |
| :--: | :-----: | :-----: | :--: | :--: | :---: | :---: | :------: |
| SDK  |  411KB  |    6    | 320  | 240  |  505  |  556  |    25    |
| 密友圈  | 33.4MB  |    6    | 2160 | 3840 | 46153 | 46259 |          |
| 和飞信  |  205KB  |    6    | 240  | 320  |  156  |  257  |    29    |
|  微信  | 888.9KB |    6    | 544  | 966  | 1326  | 1394  |    29    |


  注：比特率影响的是视频的大小和质量

  ##### 录制视频参数

  视频分辨率：320 * 240

  比特率：400

  帧率 ：25帧/s

  视频输出格式：MediaRecorder.OutputFormat.AAC_ADTS

  音频率：AudioSource.MIC

  音频录制格式 ：MediaRecorder.AudioEncoder.AAC

  音频频道 ：2 (立体声)

  音频比特率：44100


[1]: http://static.zybuluo.com/darren6/5xquvbieqa2lb8co4bdr6jhd/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20171113132910.png















