#Android SDK 集成指南

##使用提示

本文是JMLink Android SDK 标准的集成指南文档。用以指导 SDK 的使用方法，默认读者已经熟悉IDE（Eclipse 或者 Android Studio）的基本使用方法，以及具有一定的 Android 编程知识基础。

匹配的 SDK 版本为：v1.0.0及以后版本。

+ 如果您想要快速地测试、请参考本文在几分钟内跑通Demo。
+ 极光魔链文档网站上，有相关的所有指南、API、教程等全部的文档。包括本文档的更新版本，都会及时地发布到该网站上。



###Android SDK 版本

目前SDK只支持Android 2.3或以上版本的手机系统。

###JMLink 压缩包简介
+ example/
   + 一个用来展示JMlink基本用法的demo应用
+ libs/jcore-android.2.X.Y.jar
   + 极光开发者服务的核心包
+ libs/JMlink-Android-SDK-v1.X.Y.jar
   + JMLink SDK开发包
+ libs/(cpu-type)/libjcore2xy.so
   + 各种CPU类型的native开发包

###添加SDK到工程中

**选择 1: jcenter 自动集成**
+ 确认android studio的 Project 根目录的主 gradle 中配置了jcenter支持。（新建project默认配置就支持）

~~~
buildscript {
    repositories {
        jcenter()
    }
    ......
}

allprojects {
    repositories {
        jcenter()
    }
}
~~~
+ 在 module 的 gradle 中添加依赖和AndroidManifest的替换变量

~~~
android {
    ......
    defaultConfig {
        applicationId "com.xxx.xxx" //JPush上注册的包名.
        ......

        ndk {
            //选择要添加的对应cpu类型的.so库。
            abiFilters 'armeabi', 'armeabi-v7a', 'armeabi-v8a'
            // 还可以添加 'x86', 'x86_64', 'mips', 'mips64'
        }

        manifestPlaceholders = [
            JPUSH_PKGNAME : applicationId,
            JPUSH_APPKEY : "你的appkey", //极光开发平台上注册的包名对应的appkey.
            JPUSH_CHANNEL : "developer-default", //暂时填写默认值即可.
        ]
        ......
    }
    ......
}

dependencies {
    ......

    compile 'cn.jiguang.sdk:jmlink:1.0.0'  // 此处以JMlink 1.0.0 版本为例。
    compile 'cn.jiguang.sdk:jcore:2.1.2'  // 此处以JCore 2.1.2 版本为例。
    ......
}
~~~

***注：***如果在添加以上 abiFilter 配置之后android Studio出现以下提示：
~~~
NDK integration is deprecated in the current plugin. Consider trying the new experimental plugin.
~~~
则在 Project 根目录的gradle.properties文件中添加：
~~~
 android.useDeprecatedNdk=true
~~~

**选择 2：手动导入**

#### 解压SDK并导入
+ 在极光官网下载最新 SDK
+ 解压缩 JMLink-Android-SDK-v1.X.Y.zip 集成压缩包
+ 复制 libs/jcore-android_2.X.Y.jar 到工程 libs/ 目录下。
+ 复制 libs/JMlink-Android-SDK-v1.X.Y.jar 到工程 libs/ 目录下。
+ 复制 libs/(cpu-type)/libjcore1xy.so 到你的工程中存放对应cpu类型的目录下。

***说明1: ***使用android studio的开发者，如果使用jniLibs文件夹导入so文件，则仅需将所有cpu类型的文件夹拷进去；如果将so文件添加在module的libs文件夹下，注意在module的gradle配置中添加一下配置：
~~~
    android {
        ......
        sourceSets {
            main {
                jniLibs.srcDirs = ['libs']
                ......
            }
            ......
        }
        ......
    }
~~~

#### 配置 AndroidManifest.xml
- 添加权限
~~~
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS"/>
    <uses-permission android:name="android.permission.WAKE_LOCK" />
~~~

- 添加极光魔链Appkey信息
~~~
        <meta-data
            android:name="JPUSH_APPKEY"
            android:value="您自己的appkey" /> <!-- </>值来自开发者平台取得的AppKey -->
        <meta-data
            android:name="JPUSH_CHANNEL"
            android:value="default_developer" />
~~~

### JMLink 混淆
+ 请在工程的混淆文件中添加以下配置：

```
-dontwarn cn.magicwindow.**
-keep class cn.magicwindow.** {*;}

-dontwarn cn.jiguang.**
-keep class cn.jiguang.** { *; }
```

##更多API

其他 API 的使用方法请参考接口文档：[Android SDK API](./android_api)

##运行Demo

压缩包附带的 example 是一个 API 演示例子。你可以将它导入到你的工程，并将你的
AppKey 填入到 demo 的 build.gradle 中，设置上applicationId然后直接运行起来测试。

## 技术支持

邮件联系：[support&#64;jpush.cn](mailto:support&#64;jpush.cn)

