# Web SDK API

## JMLink 初始化

- **_new JMLink(config)_** 

- 接口说明： 用与绑定后台配置跳转应用参数
- 参数说明： config 为初始化参数，配置可以传数组。
- 调用示例：

```
new JMLink({
    jmlink: "短链",
    button: document.querySelector('a#btnOpenApp'),
    autoLaunchApp : false,
    downloadWhenUniversalLinkFailed: true,
    inapp : false,
    params: {}
  });
```

## 初始化参数 config 说明

### config.jmlink(必填)

- 类型：String
- 描述：由后台分配，短链地址或短链接后缀码(建议使用)。如您的短链接是"https://xxxxx.jmlk.co/AAaH",则对应的短链接后缀码为 AAaH。

### config.button(必填)

- 类型：HTMLAnchorElement (即 a 标签)
- 描述：打开 APP 的 a 标签, 一般会使用 CSS 定义 a 标签的外观, 使其看起来像一个按钮;
- 注意：config.button 是必要参数并且只能是 a 标签, 不能是 button、input 或其他 HTML 元素, 否则 JMlink 无法初始化成功。为什么 button 是必要参数? 因为很多浏览器禁止通过 JS 直接打开 APP, 必须通过用户手动点击才能打开应用。

### config.autoLaunchApp

- 类型：Boolean，默认 false
- 描述：此选项配置为 true 时, 用户打开您集成了 jmlink 的页面时, 在支持自动打开 APP 的浏览器和设备中, 将自动唤起 APP; 但遗憾的是有一些浏览器因为限制通过 JS 直接唤起 APP, 即使此选项设为 true 也不能自动打开应用, 此时需要用户手动点击由 config.button 指定的链接才能打开 APP;
- 注意：此选项设置为 true 此时会出现以下情况, 请您自行斟酌是否有必要设置为 true：

  - 在 Android 以及 iOS 低于 9.0 的系统中, 微信、QQ、QQ 浏览器, 如果在极光魔链后台的 mLink 高级设置中开启了"应用宝跳转", 可以自动唤起应用, 如果没有开启"应用宝跳转", 会重定向到一个"请在浏览器中打开"的引导页面;
  - 在微博内, 无论是否安装过 APP, 都会重定向到一个"请在浏览器中打开"的引导页面;
  - 在当前设备未安装 APP 时, iOS 9+ 的 Safari 会提示: "Safari 打不开该网页, 因为该网址无效。", 此提示无法通过 JS 代码阻止;
  - 在极光魔链后台开启了 Universal Link 开关时, 不能自动唤起 APP, 因为 Universal Link 必须由用户点击链接才能唤起 APP;
  - Android 系统的 Chrome、QQ(不是 QQ 浏览器)和部分基于 Chrome 内核的浏览器, 不能自动唤起应用, 因为这些浏览器或 Web 容器需要用户手动点击才能唤起 APP;
  - 在已安装 APP 情况下, UC、QQ 浏览器和 iOS 9+的 Safari 内, 在您的页面加载完成后会自动弹出"是否打开 APP"的对话框, 用户点击确认按钮后能打开 APP, 点击取消按钮后会无效;
  - 在一些不支持 Scheme 和 Universal Link 的浏览器或 Web 容器内, 都会因为无法打开 APP 而引导用户去往下载地址; 如何引导用户下载安装根据 options.autoRedirectToDownloadUrl 的选项走不同的流程。


### config.downloadWhenUniversalLinkFailed

- 类型：Boolean，默认 true
- 描述：用于在 Universal Link 打开失败时, 直接跳到下载页面

### config.inapp

- 类型：Boolean，默认 false
- 描述：当您的 HTML 页面被嵌入在 APP 中的 Webview 时, 因为 APP 中调用的 Webv View 与常规浏览器的功能有差异, jmlink 所依赖的 Universal Link 在 Webview 中会有缺陷, 但是 jmlink 又无法准确检测当前打开的环境是 Web iew 还是 APP, 所以,如果在 APP 中的 Web View 使用 JMLink 时, 为了确保 jmlink 能在 iOS 9.0 以上系统中的 APP 也能正确唤起指定的 APP 界面, 请将此选项设置为 true, 设为 true 时, jmlink 会优先使用 Custom URI Scheme 来唤起指定页面。

### config.plhparams

- 类型：Object
- 描述：传给 APP 的[占位符](/jmlink/guideline/terms/#_6)参数, 这些参数必须是您在极光魔链后台的 jmlink 服务配置中设置好的参数占位符, 否则会被过滤掉; 如果 APP 已安装, 那么唤起应用时从打开 APP 的 URI Scheme 或 Universal Link 中就能获得参数, 如果是第一次安装, 这些参数可以通过极光魔链 SDK 的 API 从我们的服务器取回; 关于 SDK 如何实现 jmlink 请参考对应android/iOS SDK 文档

+ 示例：
    + 1.在后台的 mLink 服务配置中填的 URI Scheme: myapp://path/acticle?articleId=:id
    + 2.config.plhparams:{id: '123456' }
    + 3.最终的 URI Scheme: myapp://path/acticle?articleId=123456

### config.params

- 类型：Object
- 描述：传给 APP 的参数，用户可以通过这个参数从 H5 页面传递一些参数到 APP 内进行使用。这些参数不是极光魔链后台的 jmLink 服务配置中设置的参数占位符。

```
tips
参数键值对 key - value 中，建议 key 的参数避免有重复的情况，如果 plhparams 和 params 中有相同的key，则以params 中的 key - value 值为准。
```

### config.invtparams

- 类型：Object
- 描述：邀请参数，使用这个参数追踪来源，现在支持的参数如下（非下面的参数无效）
  u_id: 来源用户 ID
- 示例：invtparams: {u_id : "uid-123456"}
