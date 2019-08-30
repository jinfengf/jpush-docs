#iOS SDK API

##SDK接口说明

1. JMLinkService，包含SDK所有接口
2. JMLinkConfig类，应用配置信息类

##初始化

+ ** setupWithConfig:(JMLinkConfig * )config;**

    + 接口说明:
        + 初始化接口
    + 参数说明
        + config 配置类
    + 调用示例: 

```
	// 如需使用 IDFA 功能请添加此代码并在初始化配置类中设置 advertisingId
	NSString *idfaStr = [[[ASIdentifierManager sharedManager] advertisingIdentifier] UUIDString];	   
	JMLinkConfig *config = [[JMLinkConfig alloc] init];
	config.appKey = @"AppKey copied from JiGuang Portal application";
	config.advertisingId = idfaStr;
	[JMLinkService setupWithConfig:config];        
```



##设置debug模式

+ ** (void)setDebug:(BOOL)enable;**
    + 接口说明:
        + 开启debug模式，开发时可以开启查看日志，准备提交上线时建议关闭
    + 参数说明
        + enable 是否开启debug模式
    + 调用示例
```
	  [JMLinkService setDebug:YES];
```

##开启Crash日志收集

+ ** (void)crashLogON;**
    + 接口说明:
        + 开启Crash日志收集，默认为关闭状态
    + 调用示例
    
```
	  [JMLinkService crashLogON];
```


##注册一个mLink handler，当接收到URL的时候，会根据mLink key进行匹配，当匹配成功会调用相应的handler
+ ** (void)registerMLinkHandlerWithKey:(NSString \*)key handler:(void (^)(NSURL \*url ,NSDictionary \*params))handler;**

    + 接口说明:
        + 注册一个mLink handler，当接收到URL的时候，会根据mLink key进行匹配，当匹配成功会调用相应的handler
    + 参数说明
        + key  mlink key
        + handler 回调block
        + url  收到的URL
        + params 解析的参数

    + 调用示例:
  
```
		[JMLinkService registerMLinkHandlerWithKey:@"key" handler:^(NSURL * _Nonnull url, NSDictionary * _Nullable params) {
		   //处理回调
		}];
```


##注册一个默认的mLink handler，当接收到URL，并且所有的mLink key都没有匹配成功，就会调用默认的mLink handler
+ ** (void)registerMLinkDefaultHandler:(void (^)(NSURL \*url ,NSDictionary \*params))handler;**

    + 接口说明:
        + 注册一个默认的mLink handler，当接收到URL，并且所有的mLink key都没有匹配成功，就会调用默认的mLink handler
    + 参数说明
        + key  mlink key
        + handler 回调block
        + url  收到的URL
        + params 解析的参数

    + 调用示例:
    
```
		[JMLinkService registerMLinkDefaultHandler:^(NSURL * _Nonnull url, NSDictionary * _Nullable params) {
		   //处理回调
		}];
```


##根据不同的URL路由到不同的app展示页

+ ** (BOOL)routeMLink:(NSURL \*)url;**

    + 接口说明:
        + 根据不同的URL路由到不同的app展示页。请在 application:(UIApplication \*)application openURL:(NSURL \*)url sourceApplication:(NSString \*)sourceApplication annotation:(id)annotation 中调用
    + 参数说明:
        + url Mlink URL

    + 调用示例:

```
	    - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{
	    	return [JMLinkService routeMLink:url];
		}
```


##根据universal link路由到不同的app展示页

+ ** (BOOL)continueUserActivity:(NSUserActivity \*)userActivity;**

    + 接口说明:
        + 根据universal link路由到不同的app展示页。请在 application:(UIApplication \*)application continueUserActivity:(NSUserActivity \*)userActivity restorationHandler:(void(^)(NSArray \*restorableObjects))restorationHandler 中调用
    + 参数说明:
        + userActivity Universal Link

    + 调用示例:

```
		- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray<id<UIUserActivityRestoring>> * _Nullable))restorationHandler{
		    return [JMLinkService continueUserActivity:userActivity];
		}
```

##获取无码邀请中传回来的相关值

+ ** (void)getMLinkParam:(NSString \*)paramKey handler:(void (^)(NSDictionary \*params))handler;**

    + 接口说明:
        + 获取无码邀请中传回来的相关值
    + 参数说明:
        + handler 结果回调
        + params 相关字段

    + 调用示例:

```		
		[JMLinkService getMLinkParam:@"mlink key" handler:^(NSDictionary * _Nullable) {
		   //处理回调
		}];
```		



##JMLinkConfig类

应用配置信息类。以下是属性说明：

|参数名称|参数类型|参数说明|
|:-----:|:----:|:-----:|
|AppKey |NSString|极光系统应用唯一标识，必填|
|channel|NSString|应用发布渠道，可选|
|advertisingId|NSString|广告标识符，可选|
|isProduction|BOOL|是否生产环境。如果为开发状态，设置为NO；如果为生产状态，应改为YES。可选，默认为NO|



<!-- ##错误码列表

|code|描述|备注|
|:-----:|:----:|:-----:| -->
