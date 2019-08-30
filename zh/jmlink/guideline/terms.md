# 名词释义

### 深度链接
| Deep Linking        | Android    |  iOS  |
    | :--------:   | :-----:   | :----: |
    | Custom URL Scheme        |   所有系统支持    |   所有系统支持    |
    | Applinks        | Android 6 +     |   -    |
    | Universal links        | -      |   iOS 9 +    |

### 延迟深度链接
用户点击投放内容，若用户并未安装app，则服务将引导用户安装应用，并在应用首次启动自动打开感兴趣的内容页面。深度链接服务作用在于，用户点击后系统会自动记录该用户的操作和设备硬件指纹信息，当用户安装app并启动后，当前设备的硬件指纹信息会传输到后台进行匹配，如果匹配成功，则进行场景还原。

### URI Scheme
URI Scheme的通用格式是: scheme:[//[user:password@]host[:port]][/]path[?query][#fragment]

推荐这样使用URI Scheme: scheme://host/path?query

查看 [URI Scheme 使用方法](../advanced/scenes/)


### Universal Links
在 WWDC 2015上，Apple为 iOS 9 推出的通用链接的 deep link特性，即Universal links。

### 短链接
集合活动内容，用于投放各类渠道的链接

### 短链服务key
短链接服务Key是链接唯一标识，用于集成客户端 SDK 时的服务路由注册。页面key只能包含英文字母,数字和下划线，且只能以英文字母或者下划线开头,且长度不得大于50

### 短链接后缀码
一条链接如 example.jmlk.co/ABCS , 其中 ABCS 是后缀码，用于集成JS sdk初始化时的入参信息。

### Web引导页
在短链接配置中，默认是下载页面。当用户在未安装App时，可以引导用户跳转到应用市场或一个Web引导页，这个Web引导页可以是活动H5站点或者其他网站

### 占位符
placeholder params，是指在短链接配置的 URI Scheme 中，允许使用占位符来为每个请求填写不同的值。例如 key=:keyplh,那么 keyplh 将会被请求的URL路径部分替换，即可替换的值。

