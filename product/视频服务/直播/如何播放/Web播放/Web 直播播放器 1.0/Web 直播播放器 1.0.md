## 功能介绍

腾讯云视频直播播放器 Web SDK 解决方案，可帮助腾讯云视频用户直接使用经过验证的视频播放能力，通过灵活的接口，快速同自有 Web 应用集成，以实现桌面应用播放功能，同时 SDK 提供在 Web 端上传视频的能力。<br>
该 SDK 所播放的文件受限于全局防盗链功能定义，详细内容请查看官网 FAQ 安全功能相关说明，该文档面向考虑使用腾讯云视频直播播放器 Web SDK 进行开发并具备 Javascript 语言基础的开发人员。

## 格式支持
播放格式：支持 Web SDK 直播视频格式。

|播放格式    | PC 浏览器   | 移动端浏览器 |
|-------------|------|--------|
| HLS（m3u8） | 支持 | 支持   |
| RTMP        | 支持 | 不支持 |
| FLV         | 支持 | 不支持   |

**Android 系统兼容性问题**：不像 iPhone 上只有一个 Safari 浏览器，Android 上的系统标配浏览器有非常多的实现版本，所以 Android 手机浏览器的兼容是一个业界难题，故此表格中所示的手机浏览器格式支持情况比不适用于所有 Android 手机。


## 准备工作

### Step 1：开通服务
在 [腾讯云官网](https://cloud.tencent.com/) 注册腾讯云帐号，并开通**直播**服务。
### Step 2：创建频道
进入 [直播管理控制台](https://console.cloud.tencent.com/live) 并创建直播频道。
### Step 3：获取 ID
您可以在直播管理控制台查找到直播频道并对该频道进行管理，在控制界面您可以找到 app_id，单击该直播频道可以在【基础设置】查询到该频道的频道 ID。
![](//mc.qcloudimg.com/static/img/ac00be44d369a9fa6bc5a93c2b837527/image.png)
![](//mc.qcloudimg.com/static/img/41ad87e5f153cb1df1d31d6b4a55ea47/image.png)

### Step 4：页面准备
在需要播放视频的页面（包括 PC 或 H5）中引入初始化脚本。

```
<script src="//qzonestyle.gtimg.cn/open/qcloud/video/live/h5/live_connect.js" charset="utf-8"></script>;
```

>**注意：**
>**播放页面需要挂 IP 或域名访问，如若直接打开本地静态页面将无法播放；**
>另外可以通过服务端 API 获取频道 ID 和 APPID，具体请参考 [文档链接](https://cloud.tencent.com/document/api/267/5664)。

## API 基础使用方法

### Step 1：添加播放器容器

 在需要展示播放器的页面位置加入播放器容器<span class="anchor" id="step1"><span id="basic_use"></span>，例如：在 index.html 中加入如下代码：

```
<div id="id_video_container" style="width:100%; height:auto;"></div>
```

容器 ID 以及宽高都可以自定义。

### Step 2：创建 Player 对象

接下来在页面底部引入的 Javascript 脚本中创建一个播放器对象，这时将使用播放器的构造函数：
```
var player = new qcVideo.Player("id_video_container", {
"channel_id": "16093104850682282611",
"app_id": "1251783441",
"width" : 480,
"height" : 320
});
```
调用构造函数将会生成一个播放器对象，并且根据 channel_id 和 app_id 找到对应的直播视频流进行播放，如果没有 channel_id 和 app_id，您也可以使用直播地址的形式进行播放，具体示例如 [API 使用案例中的 case3](#case3)，您可以使用播放器对象 player 对播放器进行控制，播放器对象的参数选项下方 API 方法总览有详细介绍。

### 完整实例代码
```
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1" />
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=0, minimum-scale=1.0, maximum-scale=1.0"/>
    <title>直播</title>
</head>
<body>
<div id="id_video_container" style="width:100%; height:auto;"></div>
<script src="//qzonestyle.gtimg.cn/open/qcloud/video/live/h5/live_connect.js" charset="utf-8"></script>
<script type="text/javascript">
    (function () {
        var player = new qcVideo.Player("id_video_container", {
            "channel_id": "16093104850682282611",
            "app_id": "1251783441",
            "width" : 480,
            "height" : 320
        });
    })()
</script>
</body>
</html>
```

## API 使用案例

使用 API 需先完成 API 基础使用方法中的 [添加播放器容器](#basic_use) 部分，添加完成后再进行 API 使用。

### Case 1：在 PC 或移动端（H5）中播放直播视频

直播 SDK 在 PC 和 H5 中的使用方式是一样的，SDK 会检测平台，自动选择最优的播放方案，例如在 PC 平台，SDK 会优先使用 Flash  播放器以适应多种视频格式的情况（需要 Flash 版本高于 10，否则将提示升级 Flash），而在移动端 H5 则会使用 video 标签进行播放（如果浏览器不支持 H5，则提示使用 QQ 浏览器），SDK 同时支持传频道 ID 或视频文件地址的方式播放。
> **注意：**
> 两种播放方式不能混合使用。

### Case 2：使用频道 ID 播放视频
```
var option = {
"channel_id": "16093425727656143421",
"app_id": "1251132611",
"width" : 480,
"height" : 320

//...可选填其他属性
};

var player = new qcVideo.Player("id_video_container", option);
```
>**注意：**
>使用频道 ID 播放视频不支持直播码的方式。

### Case 3：使用直播视频地址播放视频 
如果没有 app_id 及 channel_id<span class="anchor" id="case3"><span id="case3"></span>，播放器也支持使用直播视频地址播放视频。
```
var option = {
"live_url" : "http://2157.liveplay.myqcloud.com/2157_358535a.m3u8",
"live_url2" : "http://2000.liveplay.myqcloud.com/live/2000_2a1.flv",
"width" : 480,
"height" : 320

//...可选填其他属性
};

var player = new qcVideo.Player("id_video_container", option);
```

> **注意：**
> 最多支持传入两个播放地址，live_url、live_url2 ，如果这两个地址都传了，那么会按平台支持最好的一个地址选择进行播放，例如当前环境是 PC，那么会优先选择其中为 rtmp 或 flv 的格式，当前环境为手机 H5，会优先选择 hls 格式进行播放。

### Case 4：如何使用"弹幕"功能?
在播放器初始化完成后，调用播放器对象的 addBarrage(barrage) 方法，可以为视频添加弹幕，具体参数参考 [API 方法总览](#api) 的说明，例如，给正在播放的视频添加两条弹幕：

```
var barrage = [
{"type":"content", "content":"hello world", "time":"1"},
{"type":"content", "content":"居中显示", "time":"1", "style":"C64B03;30","position":"center"}
];
player.addBarrage(barrage);
```
>**注意：**
>**弹幕功能仅在前端实现，后台支持请自行开发，弹幕只在 PC Flash 播放器中生效，H5 暂时不具备弹幕功能。**

##  API 方法总览

### 构造函数

```
qcVideo.Player(id, option, listener);
```

 **ID**<span class="anchor" id="api"><span id="api"></span>：String，**必选**参数，页面放置播放器的容器 ID，可以自由定义；
  **option**：Object，**必选**参数，播放器的参数可设置选项，具体选项：

| 参数             | 类型   | 默认值 | 参数说明   |
|------------------|--------|--------|---------------------------------------------------------------------------------------------------|
| channel_id      | String | 无     | 用直播视频 ID 播放方式为**必选**参数                                                                    |
| app_id          | String | 无     | 条件同上为**必选**参数，同一个账户下的视频，该参数是相同的                                            |
| width            | Number | 无     |** 必选**，例如：640，设置播放器宽度，单位为像素                                                       |
| height           | Number | 无     | **必选**，例如：480，设置播放器高度，单位为像素                                                       |
| cache_time      | Number | 0.3    | 直播画面开始播放前，最大缓存时间；这个属性可避免有效下行带宽不足导致免 rtmp 直播卡顿的情况（可选参数）<br> **备注：该选项只对 PC 平台 Flash 播放器生效** |
| h5_start_patch | Object | 无     | H5 播放，开始播放前贴片（可选参数）<br>{ <br>url : 图片地址, <br>stretch: false //是否拉伸图片铺面整个播放器，默认 false<br>}                                                                                                  |
| wording          | Object | 无     | 直播提示语自定义（可选参数，详细信息可见 [错误码](https://cloud.tencent.com/document/product/267/13500)）<br>{	<br>	'1' 	: '数据库错误',<br> '2'		: '连接不到直播源，直播源没有推送视频内容(hls)',<br> '3'		: '直播已经结束啦(hls)',<br>'113'	: '连接超时，请稍后再试',<br>'114'	: '连接超时，请稍后再试',<br>'1000'	: 'channelID 或者 APPID 错误',<br>'1001'	: '无效参数，获取 bizid 失败',<br>'1009'	: '直播源已失效',<br>'10000'	: '请求超时，请检查网络设置',<br> '10008'	: '密码错误，请重新输入', //无效密码<br>'10020'	: '直播账户余额已不足，请及时充值',  <br>'11044'	: '无效请求',<br> '11045'	: '请求参数缺少 channelID',<br> '11046'	: '密码错误，请重新输入',<br> '20110'	: '密码错误，请重新输入',<br>'20113'	: '直播已经结束啦，请稍后再来' , //'downstream type is not exist' 拉流不存在<br>'20201'	: '直播已经结束啦，请稍后再来', //get upstream info error'  查询推流错误<br> '20301'	: '直播频道不存在，请确认频道 ID',<br>'TipReconnect'	: '重新连接中'<br>'TipRequireSafari'	: '当前浏览器不能支持视频播放，请使用 safari 打开观看'<br>'TipRequireFlash'	: '当前浏览器不能支持视频播放，可下载最新的 QQ 浏览器或者安装 Flash 即可播放'<br>'TipVideoinfoResolveError'	: '视频信息解析错误，请检查参数是否正确', //接口没有返回 json 数据，无法解析<br>'TipVideoinfoError'	: '视频信息错误，请检查参数是否正确',<br>'TipConnectError'	: '连接服务失败，请检查网络设置',<br>'TipConnectDeny'	: '连接服务被拒绝', //Flash 请求触发安全异常<br>'TipLiveEnd'		: '直播已经结束啦，请稍后再来',  // NetStream.Play.Stop 事件, <br>'TipStreamNotFound'	: '直播已经结束啦，请稍后再来' //连接不到直播源，直播源没有推送视频内容<br>}                                                                                                 |
| live_url        | String | 无     | 直播地址，支持 hls/rtmp/flv 三种格式<br>用视频地址播放方式为**必选参数 **                                                                     |
| live_url2       | String | 无     | 直播地址，同上（可选参数）                                                                        |
| volume           | Number | 0.5    | 设置音量初始值 0 到 1，默认 0.5（可选参数）**备注：该选项只对 Flash 播放器生效**                                                                    |
| https             | Number | 0   | 设置对播放页的 https 支持，0：关闭，1：开启                                                                    |
| hide_volume_tips  | Number | 0   | 是否隐藏音量提示，0：显示，1：隐藏 <br>**备注：该选项只对 Flash 播放器生效 **                                                                  |
| x5_type           | String | 无   | 通过 video 属性“x5-video-player-type”声明启用同层 H5 播放器，支持的值：H5 （该属性为 TBS 内核实验性属性，非 TBS 内核不支持，具体介绍参照 [TBS 文档](http://x5.tencent.com/guide?id=4000) ）                           |
| x5_fullscreen     | String | 无   | 视频播放时将会进入到全屏模式，支持的值： true：开启（该属性为 TBS 内核实验性属性，非 TBS 内核不支持，具体介绍参照 [TBS 文档](http://x5.tencent.com/guide?id=4000)）                                                   |
| WMode             | String  | window | Window 模式不支持其他页面元素覆盖在 Flash 播放器上面，如需要可以修改为 opaque 或其他 flash wmode 的参数值，可以在 Flash 播放器上进行覆盖其他元素。<br> **备注：该选项只对 PC 平台 Flash 播放器生效**                                                                  |

**listener**：Object，可选参数，播放状态变化回调函数列表。

| 函数名称                                                 | 类型     | 说明   |
|----------------------------------------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------|
| playStatus<span id="playstatus" class="anchor"></span> | function | 播放状态变更时触发，回调函数的参数 status：String <br>返回值：<br>ready: “播放器已准备就绪”, playing:”播放中” , playEnd:”播放结束” , error: “播放异常结束或者遇到错误” //由于移动端播放事件触发条件不一致，如果监听播放结束事件建议同时监听error，以免回调不能正确执行。  <br>type：String 在遇到error时返回错误类型 <br>例子：function(status, type){ ... }   |

### 设置和动作

构造函数返回的播放器对象具有以下设置方法

| 方法                 | 说明                          |
|----------------------|-------------------------------|
| resize(width,height) | 参数：width：int；height：int <br>功能：设置当前播放器宽度高度<br>返回：无                       |
| play()                                                  | 功能：开始播放<br>返回：int（统一返回码）<br>**备注：该功能仅 PC 端 Flash 播放器支持 **                                                          |
| stop()                                                  | 功能：停止播放<br>返回：int（统一返回码）<br>**备注：该功能仅 PC 端 Flash 播放器支持 **                                                                   |
| pause()                                                       | 功能：暂停当前播放的视频 <br>返回：int（统一返回码)<br>**备注：该功能仅 PC 端 Flash 播放器支持**                                                                                 |
| resume()                                                      | 功能：恢复播放视频<br>返回：int（统一返回码）<br>**备注：该功能仅 PC 端 Flash 播放器支持**       |
| addBarrage(barrage) | 参数：barrage://Array 弹幕信息   <br> \[{   <br>"type":"content", //消息类型，content：普通文本（必选）  <br>"content":"hello world", //文本消息（必选） <br>"time":"1.101",//单位秒 ，表示距离当前调用添加字幕的时间多久后，开始显示该条字幕（必选）   <br>"style": "C64B03;35",// 分号分割，第一项颜色值，第二项字体大小（可选）<br>"postion":"center" //固定位置 <br>center: 居中，bottom: 底部， up: 顶上 (可选) }, ... \]  <br>功能：添加弹幕     <br>返回：int [返回码](https://cloud.tencent.com/document/product/267/13500) <br> 备注：**弹幕仅在前端实现，后台功能请自行开发，该功能只在 PC Flash 播放器中生效 **                                                                                |
| closeBarrage()                                                | 功能：关闭弹幕，关闭后重新调用 addBarrage 可开启弹幕。 <br>返回：int [返回码](https://cloud.tencent.com/document/product/267/13500)  <br> 备注：**弹幕仅在前端实现，后台功能请自行开发，该功能只在 PC Flash 播放器中生效 **                                                                                    |

这些设置方法的统一返回码是：
  
| 错误码<span id="errorcode"></span> | 含义 |
|---------|---------|
| 200 | 操作成功 | 
| 0  | 播放器未完全初始化 | 
| -2 | 未知操作命令 | 