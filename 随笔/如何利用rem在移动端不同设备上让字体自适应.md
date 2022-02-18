## [如何利用rem在移动端不同设备上让字体自适应大小](https://www.cnblogs.com/zhuanshen/p/7098707.html)

**rem由来**：font size of the root element，那么rem是个单位，单位大小由它第一代老祖宗的**font-size**的大小决定。现在前端码农们为了能在各个屏幕上看到一个健康的网页在默默的牺牲着自己的健康，因为不仅要知道rem是个单位，更重要的是要知道怎么能在不同分辨率下呈现的页面都很NB。

事故造成原因：
1.px单位在PC上很流行，在手机屏幕上一看，MLGB的，同样的12px却小的跟蚂蚁似的。
2.好不容易在iPhone4上调的正常了，换个菊花牌手机，MBD不堪入目了。
3.知道了rem的用法，但是html的font-size到底是多少才合适啊啊啊，妈蛋~。

好了，那么现在来解决这些问题。
在解决之前，麻烦各位大婶要了解一些你可能不想了解的东东（警告：不了解这些就不能知道真相哟~）：
1.**物理像素(physical pixel)**
我们看到的每个屏幕都是由一颗颗我们肉眼难以看到的小颗粒（物理像素）组成的。



**2.逻辑像素**
是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素(比如说CSS像素)。

**3.设备的像素比(device pixel ratio)简称DPR**
它的数值体现了物理像素和逻辑像素之间的关系，用公式可以计算出该设备的DPR的大小：

```
DPR = 物理像素 / 逻辑像素
```

那么了解了上面这些概念，就可以知道，为什么css在pc上写着`font-size=12px;`但是换到手机上却变小了？因为DPR啊啊啊，大哥~。
没错，我们在电脑屏幕上的DPR是1，但是手机却不同，可能是它可能是2，也可能是3。获取设备DPR的方法还是有的：
1.在JavaScript中，通过`window.devicePixelRatio`来获取
2.在css中，可以通过`-webkit-device-pixel-ratio`，`-webkit-min-device-pixel-ratio`和 `-webkit-max-device-pixel-ratio`进行媒体查询，对不同DPR的设备，做一些样式适配(这里只针对webkit内核的浏览器和webview)。

本人也在网上看了不少动态设置rem的文章，下面把几个常用的列举出来：一，用媒体查询来设置html的font-size：

```
@media screen and (min-width: 320px) {
    html {font-size: 14px;}
}
 
@media screen and (min-width: 360px) {
    html {font-size: 16px;}
}
 
@media screen and (min-width: 400px) {
    html {font-size: 18px;}
}
 
@media screen and (min-width: 440px) {
    html {font-size: 20px;}
}
 
@media screen and (min-width: 480px) {
    html {font-size: 22px;}
}
 
@media screen and (min-width: 640px) {
    html {font-size: 28px;}
}
```

二、利用js来动态设置

```
!(function(doc, win) {
    var docEle = doc.documentElement,
        evt = "onorientationchange" in window ? "orientationchange" : "resize",
        fn = function() {
            var width = docEle.clientWidth;
            width && (docEle.style.fontSize = 20 * (width / 320) + "px");
        };
     
    win.addEventListener(evt, fn, false);
    doc.addEventListener("DOMContentLoaded", fn, false);
 
}(document, window));
```

我要说的是最后一种，也是我认为目前比较好的实现方法：
利用js计算当前设备的DPR，动态设置在html标签上，并动态设置html的`font-size`，利用css的选择器根据DPR来设置不同DPR下的字体大小（这个方法很不错哦~）

```
!function(win, lib) {
    var timer,
        doc     = win.document,
        docElem = doc.documentElement,

        vpMeta   = doc.querySelector('meta[name="viewport"]'),
        flexMeta = doc.querySelector('meta[name="flexible"]'),
 
        dpr   = 0,
        scale = 0,
 
        flexible = lib.flexible || (lib.flexible = {});
 
    // 设置了 viewport meta
    if (vpMeta) {
 
        console.warn("将根据已有的meta标签来设置缩放比例");
        var initial = vpMeta.getAttribute("content").match(/initial\-scale=([\d\.]+)/);
 
        if (initial) {
            scale = parseFloat(initial[1]); // 已设置的 initialScale
            dpr = parseInt(1 / scale);      // 设备像素比 devicePixelRatio
        }
 
    }
    // 设置了 flexible Meta
    else if (flexMeta) {
        var flexMetaContent = flexMeta.getAttribute("content");
        if (flexMetaContent) {
 
            var initial = flexMetaContent.match(/initial\-dpr=([\d\.]+)/),
                maximum = flexMetaContent.match(/maximum\-dpr=([\d\.]+)/);
 
            if (initial) {
                dpr = parseFloat(initial[1]);
                scale = parseFloat((1 / dpr).toFixed(2));
            }
 
            if (maximum) {
                dpr = parseFloat(maximum[1]);
                scale = parseFloat((1 / dpr).toFixed(2));
            }
        }
    }
 
    // viewport 或 flexible
    // meta 均未设置
    if (!dpr && !scale) {
        // QST
        // 这里的 第一句有什么用 ?
        // 和 Android 有毛关系 ?
        var u = (win.navigator.appVersion.match(/android/gi), win.navigator.appVersion.match(/iphone/gi)),
            _dpr = win.devicePixelRatio;
 
        // 所以这里似乎是将所有 Android 设备都设置为 1 了
        dpr = u ? ( (_dpr >= 3 && (!dpr || dpr >= 3))
                        ? 3
                        : (_dpr >= 2 && (!dpr || dpr >= 2))
                            ? 2
                            : 1
                  )
                : 1;
 
        scale = 1 / dpr;
    }
 
    docElem.setAttribute("data-dpr", dpr);
 
    // 插入 viewport meta
    if (!vpMeta) {
        vpMeta = doc.createElement("meta");
         
        vpMeta.setAttribute("name", "viewport");
        vpMeta.setAttribute("content",
            "initial-scale=" + scale + ", maximum-scale=" + scale + ", minimum-scale=" + scale + ", user-scalable=no");
 
        if (docElem.firstElementChild) {
            docElem.firstElementChild.appendChild(vpMeta)
        } else {
            var div = doc.createElement("div");
            div.appendChild(vpMeta);
            doc.write(div.innerHTML);
        }
    }
 
    function setFontSize() {
        var winWidth = docElem.getBoundingClientRect().width;
 
        if (winWidth / dpr > 540) {
            (winWidth = 540 * dpr);
        }
 
        // 根节点 fontSize 根据宽度决定
        var baseSize = winWidth / 10;
 
        docElem.style.fontSize = baseSize + "px";
        flexible.rem = win.rem = baseSize;
    }
 
    // 调整窗口时重置
    win.addEventListener("resize", function() {
        clearTimeout(timer);
        timer = setTimeout(setFontSize, 300);
    }, false);
 
     
    // 这一段是我自己加的
    // orientationchange 时也需要重算下吧
    win.addEventListener("orientationchange", function() {
        clearTimeout(timer);
        timer = setTimeout(setFontSize, 300);
    }, false);
 
 
    // pageshow
    // keyword: 倒退 缓存相关
    win.addEventListener("pageshow", function(e) {
        if (e.persisted) {
            clearTimeout(timer);
            timer = setTimeout(setFontSize, 300);
        }
    }, false);
 
    // 设置基准字体
    if ("complete" === doc.readyState) {
        doc.body.style.fontSize = 12 * dpr + "px";
    } else {
        doc.addEventListener("DOMContentLoaded", function() {
            doc.body.style.fontSize = 12 * dpr + "px";
        }, false);
    }
  
    setFontSize();
 
    flexible.dpr = win.dpr = dpr;
 
    flexible.refreshRem = setFontSize;
 
    flexible.rem2px = function(d) {
        var c = parseFloat(d) * this.rem;
        if ("string" == typeof d && d.match(/rem$/)) {
            c += "px";
        }
        return c;
    };
 
    flexible.px2rem = function(d) {
        var c = parseFloat(d) / this.rem;
         
        if ("string" == typeof d && d.match(/px$/)) {
            c += "rem";
        }
        return c;
    }
}(window, window.lib || (window.lib = {}));
```

忘了说了，手机淘宝很多页面用的就是这种方法来适配终端的。

然后留个demo，可以看一下手机淘宝用上面方法怎么实现的（用手机扫下面的二维码吧）：

[或点击去查看](http://huodong.m.taobao.com/act/yibo.html)

**结束语：**
其实想要适配好不同终端并不难，一定要理解好什么是物理像素、什么是逻辑像素、什么是DPR，并且弄清楚他们之间的关系，我们这些搞前端的，不就是在与像素打交道吗？



转自https://www.cnblogs.com/zhuanshen/p/7098707.html