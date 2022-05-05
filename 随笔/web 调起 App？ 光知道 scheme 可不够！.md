很多人会遇到这个问题，大多数都是站在移动端的角度去解释这个问题，本篇文章将会从**web端**的角度分析问题，解决问题

#### 背景

###### 了解背景，有助于我们更加深入的理解需求，分析需求

用户和流量的量级决定了一款互联网产品的优秀度。对一款优雅的互联网产品而言，app端和web端是必不可少的，我们借助它们形成闭环，保证用户体验和活跃度。除了端的覆盖，我们还要做到端到端的交互，由此我们提出了产品的需求： **wap页唤起app，如果用户没装app则跳转到下载页(引导页都行)。如果用户已安装，根据URL跳到app内指定的界面**，这也正是本文正要解决的问题

#### 1. 先说一个最常规的方法，也是最常用的方法：scheme

举个🌰：

```
wzry://videolists.show/mark/penta_kill?num=1
复制代码
```

这种方式需要事先在app中注册相应的scheme，通过网页访问已注册的scheme，达到调起App客户端及相关界面

##### Android的相关配置： AndroidManifest.xml

```
<activity android:name=".ui.SchameFilterActivity">
	<intent-filter>
	<action android:name="android.intent.action.VIEW" />
	<category android:name="android.intent.category.DEFAULT" />
	<category android:name="android.intent.category.BROWSABLE" />
	<data
	    android:host="videolists.show"
	    android:path="/mark/penta_kill"
	    android:scheme="wzry"/>
	</intent-filter>
</activity>
复制代码
```

##### iOS 图像化配置还说啥



![img](https://raw.githubusercontent.com/sady-19/MdImage/main/img/202203281554713.webp)



###### 至于定义scheme以后的逻辑请自行[google](https://link.juejin.cn?target=https%3A%2F%2Fwww.google.com)，但是逻辑中一定要做防护

好了我们可以愉快的使用了

```
<a href="wzry://videolists.show//mark/penta_kill?vid=666">你过来啊！</a>
复制代码
```

（你也用直接访问地址的方式，这种方式有小坑：有些浏览器是会返回404状态码，或者变成搜索结果页，小烦）

声明sheme后，回过头来看一下需求：用户安装了app，调起app。用户未安装我们需要引导用户下载 。那这里就有一个问题了：**网页是不知道用户有没有安装app的**。那我们换一个思路----可不可以知道页面被置于后台了？页面进入后台，可以认为是唤起成功了；页面没进入后台，说明唤起失败(没安装or其他原因)

一个新的API完全满足上边提到的需求 : [**Page Visibility API**](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FPage_Visibility_API)，文档中详细的介绍了这个API---->可以准确的知道页面当前的状态(前台，后台等状态)，**但是**这个API太新了，需要较新的浏览器支持，覆盖率有点低。放一张图感受一下：



![img](https://raw.githubusercontent.com/sady-19/MdImage/main/img/202203281555592.webp)

所以我们不能大面积使用。如之奈何？交给黑科技了： **定时器**



我先说一下思路：

- setTimeout的实现过程：

  浏览器尝试打开URL scheme并记录时间点t1，在2秒计时后，检查当前时间t2，如果t2-t1 > 2200ms,说明唤起app成功(唤起app会是浏览器的定时器延后执行)，如果t2-t1 < 2200ms,可能没有安装app，可以引导用户进入下载页

- setInterval实现过程

  原理上跟setTimeout相似，方法上换成设置一个比较小的时间间隔（例如20ms），运行多次(例如100),比较运行完100次的总耗时与20*100的时间差。逻辑判断同setTimeout

优缺点：

- setTimeout不稳定，因为Android是基于Linux分时多任务的，setTimeout的基准偏差不会那么大，比较难调教
- setInterval要好一点，页面一直处于前台(调起app失败)，则100次跑完，总耗时与 100x20=2000ms不会有太大差异，但页面在后台运行时(调起app成功)，此时间会明显超过2000ms

#### 注：不知道setTimeout，setInterval为什么会延迟执行的，请恶补一波它们的执行原理

好了嘴炮打完，上🌰：

**setTimeout**

```
var openTime = +new Date();
window.location.href = 'wzry://videolists.show/mark/penta_kill?num=1'
var timer = setTimeout(function () {
    if ((new Date()) - openTime < 2200) {//加了200ms基准误差
        window.location.href = 'you app download page';
    }
    if ((new Date()) - openTime > 2200) {
        clearTimeout(timer);
    }
}, 2000);
复制代码
```

**setInterval**

```
var limit_num = 100;
var openTime = +new Date();
window.location.href = 'wzry://videolists.show/mark/penta_kill?num=1'
var timer = setInterval(function () {
   if(limit_num > 0){
        limit_num--;
   }else{
        if ((new Date()) - openTime < 2200) {//加了200ms基准误差
            window.location.href = 'you app download page';
        }
        clearTimeout(timer);
   }
}, 20);
复制代码
```

当我们完成这些操作之后，在一般环境下是可以正常唤起app的了

#### 2. 那我再说一下特殊情况 ----微博，微信等

像这种爸爸级别的应用是有scheme白名单的，只有白名单内的scheme才不会被过滤掉，就问你怕不怕？嗯？

像这种我们怎么去解决那？

#### (I). 其实最简单的办法就是像爸爸低头----使用应用宝。

使用它颁发给你的应用地址，向这个地址跳转，然后一切就交给爸爸了，直接无视微信什么的，直接带你飞到app内

#### (II). 使用 Universal Links （Universal Links 在微信中已经唤不起来App了）

它是iOS9推出的一项强大的功能。可以让你 **通过http链接的方式唤起你的app(不管你的在微信/微博还是其他app内，因为Universal Links是由系统直接处理的)**，下面我来简单说一下怎么使用Universal Links完成唤醒的app，你也可以通过[官方文档](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.apple.com%2Flibrary%2Fcontent%2Fdocumentation%2FGeneral%2FConceptual%2FAppSearch%2FUniversalLinks.html)详细的了解它。

首先

- 必须给**域名配置https**才能使用它
- 在开发者中心做配置：找到对应的App ID ——> Associated Domains，设置成Enabled
- 打开工程，在capabilities中设置Associated Domains，在其中的Domains中填入你想支持的域名，注意必须以applinks:为前缀
- 编辑apple-app-site-association文件(文件名，json格式不能改变，注意apple-app-site-association文件是没有格式后缀的)

```
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "TeamID + BundleId TeamID",
                "paths": [ "/mark/penta_kill?num="]
            }
        ]
    }
}
复制代码
```

- 上传该文件到域名的根目录，确保可以被访问

做完这些，你就可以尝试唤起你的iOS app了

这里还要强调一下 **拦截的地址 一定要与当前webview的url 不在同一个域名下。这样Universal Links才会生效**，不要问我怎么知道的，都是血泪啊

#### 3. 还有最后一种 ---- 自己做引导，引导用户浏览器打开，绕过限制。不过这样，步骤较多，折损率可能会升高

综上，**Android app除了使用应用宝跳转唤起和引导页没发现其他更好的办法，iOS方案多了个Universal Links**，这也是为什么我们经常看到有些app在微信/微博中 iOS端可以直接唤起app，而Android端经常是需要走一道应用宝的原因

好了，以上内容就是我对唤起app的一些总结和经验，希望可以帮助到你

#### 未经本人允许，不得转载。文章有疏漏浅薄之处，请各位大神斧正

**修改 : 经过验证微信添加了一个屏蔽Universal Links的爆炸功能，大致原理参考：[Prevent universal links from opening in `WKWebView`/`UIWebView` ](https://link.juejin.cn?target=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F38450586%2Fprevent-universal-links-from-opening-in-wkwebview-uiwebview)**

微信内置浏览器想怎么改就怎么改，厉害厉害


作者：Gladyu
链接：https://juejin.cn/post/6844903588364288014
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。