#iOS SDK 集成指南

##使用提示

本文是JMLink iOS SDK 标准的集成指南文档。

匹配的 SDK 版本为：v1.0.0及以后版本。

+ 如果您想要快速地测试、请参考本文在几分钟内跑通Demo。
+ 极光魔链文档网站上，有相关的所有指南、API、教程等全部的文档。包括本文档的更新版本，都会及时地发布到该网站上。

##产品说明

极光魔链致力于在全民创业、App运营举步维艰的大环境下，通过技术手段为创业者提供全端的App增长解决方案。


###主要场景：

* 企业级的深度链接解决方案mLink
* 场景式连接/服务连接
* 跨App Store/应用市场的渠道分析

###iOS SDK 版本

目前SDK只支持iOS 7以上版本的手机系统。

##接入配置

###添加SDK到工程中

**选择 1: Cocoapods 导入**

+ 通过 Cocoapods 下载地址：

	~~~
    pod 'JMLink'
	~~~

注：如果无法导入最新版本，请执行 pod repo update master 这个命令来升级本机的 pod 库，然后重新 pod 'JMLink'

+ 如果需要安装指定版本则使用以下方式（以1.0.0版本为例）：

	~~~
    pod 'JMLink', '1.0.0'
	~~~

**选择 2：手动导入**

+ 在极光官网下载最新 SDK
+ 请在自己的工程中导入libs文件夹下的SDK文件:

    * jmlink-ios-x.x.x.a  jmlink版本 1.0.0及其以上

+ 为工程添加相应的Frameworks，需要为项目添加的Frameworks如下

    * AdSupport.framework（获取 IDFA 需要；如果不使用 IDFA，请不要添加）
    * CoreLocation.framework
    * CFNetwork.framework
    * CoreFoundation.framework
    * libresolv.tbd
    * libz.tbd
    * libc++.1.tbd
    * CoreTelephony.framework
    * SystemConfiguration.framework
    * Security.framework
    * CoreGraphics.framework
    * libsqlite3.tbd
    * MobileCoreServices.framework
    * jcore-ios-x.x.x.a  jcore版本 2.0.0及其以上
    * jmlink-ios-x.x.x.a

##接入代码

请将以下代码添加到引用JMLinkService.h头文件的的相关类中

~~~
    //引入JMLinkService.h头文件
    #import "JMLinkService.h"
    // 如果需要使用 idfa 功能所需要引入的头文件（可选）
	#import <AdSupport/AdSupport.h>
~~~

接入的JMLink SDK的应用，必须先初始化JMLinkService,否则将会无法正常使用，请将以下代码添加到合适的位置

~~~
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

 // 如需使用 IDFA 功能请添加此代码并在初始化配置类中设置 advertisingId
    NSString *idfaStr = [[[ASIdentifierManager sharedManager] advertisingIdentifier] UUIDString];
    
    JMLinkConfig *config = [[JMLinkConfig alloc] init];
    config.appKey = @"your appkey";
    config.advertisingId = idfaStr;
    [JMLinkService setupWithConfig:config];
}
~~~

##mLink的设置

mLink，可以将App中具体的内容页面作为一个服务，以链接的形式分享出来，通过这个链接，可以从外部唤起App并自动进入App中的指定服务页。

###mLink基础配置
***1.配置App的URL Scheme***

iOS系统中App之间是相互隔离的，通过URL Scheme，App之间可以相互调用，并且可以传递参数。

选中Target->Info->URL Types，配置URL Scheme（比如：jmlink）

![jmlink-ios][4]

在Safari中输入URL Scheme://（比如：jmlink://）如果可以唤起App，说明该URL Scheme配置成功。

***2.配置Universal link***

Universal link是iOS9的一个新特性，通过Universal link，App可以无需打开Safari，直接从微信等应用中跳转到App，真正的实现一键直达。如果使用URL Scheme的话，需要先打开Safari，用户体验变得很差，如果App未安装，还会出现以下错误对话框

![jmlink-ios][5]

所以我们强烈推荐配置Universal link，而通过极光魔链来配置Universal link也非常的简便。

2.1 配置developer.apple.com的相关信息

登录[https://developer.apple.com](https://developer.apple.com)，选择Certificate, Identifiers & Profiles，选择相应的AppID，开启Associated Domains

![jmlink-ios][0]

<div style="font-size:15px; color:#ff525e; font-weight:Bold">
注意：当AppID重新编辑过之后，需要更新相应的provisioning Profiles文件。
</div>

2.2 在portal后台填写应用的mLink配置信息

![jmlink-ios][6]

Team ID来自您的苹果开发账号，以下相应页面可以获取

![jmlink-ios][2]

2.3 配置Xcode

在Xcode工程中选择相应的target，点击Capablities，开启Associated Domains，在里面添加极光魔链的域名（比如：applinks:xxx），域名的具体指在portal后台获取，请<font style="color:#ff525e">注意每次新注册的App分配的极光魔链域名可能不一样</font>。如图：

![jmlink-ios][3]

***3.配置相关代码***

3.1 通过URL Scheme和Universal link唤起app

示例代码如下：

~~~
//iOS9以下，通过url scheme来唤起app
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
    //必写
    [JMLinkService routeMLink:url];
    return YES;
}

//iOS9+，通过url scheme来唤起app
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(nonnull NSDictionary *)options
{
    //必写
 	[JMLinkService routeMLink:url];
    return YES;
}

//通过universal link来唤起app
- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray * _Nullable))restorationHandler
{
    //必写
    return [JMLinkService continueUserActivity:userActivity];
}
~~~

<div style="font-size:15px; color:#ff525e; font-weight:Bold">
注意：当需要在openURL这个方法中处理第三方回调的时候（比如支付宝、微信回调等），请注意区分，例如以下示例：
</div>

~~~
if(支付宝回调) {
	return 支付宝回调处理逻辑;
}
else if(微信回调) {
	return 微信回调处理逻辑;
}
else if(其他第三方回调) {
	return 其他第三方回调处理逻辑;
}
else {
	[JMLinkService routeMLink:url]; 
	return YES;
}
~~~

3.2 配置深度链接，一键直达具体页

本模块实现的功能是通过深度链接跳转到app内的详情页面，若想要使用如下功能，请务必将mlink基本配置部分全部实现。

![jmlink-ios][1]

在AppDelegate中的didFinishLaunchingWithOptions方法中调用

~~~
/**
 * 注册一个mLink handler，当接收到URL的时候，会根据mLink key进行匹配，当匹配成功会调用相应的handler
 * 需要在 AppDelegate 的 didFinishLaunchingWithOptions 中调用
 * @param key 后台注册mlink时生成的mlink key
 * @param handler mlink的回调
 */
+ (void)registerMLinkHandlerWithKey:(nonnull NSString *)key handler:(void (^_Nonnull)(NSURL * __nonnull url ,NSDictionary * __nullable params))handler;
~~~

示例代码如下：

~~~
[JMLinkService registerMLinkHandlerWithKey:@"mLinkKey" handler:^(NSURL *url, NSDictionary *params) {
  //自行处理跳转逻辑，示例如下：
  ViewController *rootVC = (ViewController *)self.window.rootViewController;
  CategoryDetailViewController *detailVC = [rootVC.storyboard instantiateViewControllerWithIdentifier:@"CategoryDetailView"];
  detailVC.name1 = params[@"name1"];
  detailVC.name2 = params[@"name2"];
  detailVC.name3 = params[@"name3"];
  [rootVC pushViewController:detailVC animated:YES];
}];
~~~

3.3 场景还原

场景还原：用户下载后无需任何操作，直达指定内页，大幅提升转化率

mlink基础功能中包换了场景还原，实现了“一键唤起具体页”功能即可实现场景还原，仅需在处理跳转逻辑的时候特殊处理下启动页或者登录事件。

##更多API

其他 API 的使用方法请参考接口文档：[iOS SDK API](../ios_api/)

##运行Demo

压缩包附带的 demo 是一个 API 演示例子。你可以将它导入到你的工程，并将你的
AppKey 填入到 demo 的 AppDelegate 中，设置上BundleID然后直接运行起来测试。

## 技术支持

邮件联系：[support&#64;jpush.cn](mailto:support&#64;jpush.cn)

[0]: ../image/img-associate-domain.png
[1]: ../image/img-mlink-key.png
[2]: ../image/img-team-id.png
[3]: ../image/img-universal-link.png
[4]: ../image/img-url-scheme.png
[5]: ../image/img-web-invaild.png
[6]: ../image/img-portal-config.png
