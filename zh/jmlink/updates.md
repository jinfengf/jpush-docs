# 最近更新
### JMLink iOS SDK v1.0.0

#### 更新时间
+ 2019-08-29

#### Change Log
+ JMLink 对接 Jcore，SDK 模块化服务，可搭配极光开发者多种产品服务
+ 适配魔链新版本上报协议
+ 增加接口类为“JMLink”关键字
+ 支持scheme跳转、场景还原、应用宝跳转、无码邀请应用场景。
+ 兼容老版本魔链 SDK ，老版本接口全部标注过时，并且桥接到新版sdk接口中，保障开发者平滑升级，建议全都使用新接口！


#### 升级提示

+ 建议升级！
+ 新老用户查看[「魔链产品简介 - 快速开始」指引文档](/jmlink/guideline/intro/)

#### 升级指南
+ 若由老版本魔链 SDK 升级上来，Lib目录内头文件及库有变动，需要删除原MagicWindow.bundle、mwSDK-ios-xxx.a文件等文件，重新导入jmlink-ios-xxx.a、JMLinkService.h等文件
+ 依赖库有变动，建议删除原导入库，重新导入新的依赖库（[详见集成文档](/jmlink/client/iOS/ios_guide/)）
+ 新版中存留了部分 mlink 接口，部分接口已被废弃，建议全都使用新接口！


### JMLink Android SDK v1.0.0

#### 更新时间

+ 2019-08-29


#### ChangeLog
+ JMLink Android SDK 首次发布
+ 去掉原魔窗中与deeplink无关服务,仅保留scheme跳转、场景还原、应用宝跳转、无码邀请业务
+ 兼容老版本魔链sdk,保障开发者平滑升级,建议全都使用新接口！***注意新版本SDK中移除了非相关深度链接服务的UserProfile类及其相应的方法，如开发者在早期项目中有使用到UserProfile类和相关方法，请移除。***

#### 升级提示
+ 建议升级！

#### 升级指南
+ 首先解压您获取到的 zip 压缩包

+ 更新库文件
	+ 打开libs文件夹。添加jcore-android-2.1.2.jar 和jmlink-android-1.0.0.jar 到项目中的libs，并删除libs原有的魔窗jar文件
	+ 复制 libs/(cpu-type)/libjcore212.so 到你的工程中存放对应cpu类型的目录下。

+ 更新AndroidManifest.xml
	+ 请参考 SDK下载包最新版本的 demo 来更新AndroidManifest.xml 文件配置。
	+ 如果使用到原魔窗的一些配置可以删除掉，如MW_APPID
+ 如果使用jcenter的方式集成JMLink，详细集成说明请参考官方[集成指南](/jmlink/client/Android/android_guide/)


### JMLink Web SDK v1.0.0

#### 更新时间

+ 2019-08-29

#### Change Log
+ SDK关键词为 JMLink 和 jmlink
+ 增加初始化方法
+ 支持三种参数传递方式 


#### 升级提示

+ 建议升级！

#### 升级指南

+ 引入 JS SDK 链接的链接："https://static.jmlk.co/scripts/dist/jmlink.min.js