---
typora-copy-images-to: media
---

## Math对象

### Math常用API

随机数：math.random  得到一个随机小数，范围0-1，并且会包含0，但是不包含1

向上取整：math.ceil（数字）

Var res = math.ceil(10/3) -->4

向下取证：math.floor(数字)

四舍五入：math.round（数字）

求最大值：math.max(多个数字，逗号隔开)

求最小值：math.min（多个数字，逗号隔开）

求绝对值：math.abs（数字）

圆周率：math.PI



### 各进制间数字的转换

tostring方法将十进制数字转为其他进制

```js
var  x = 110;
x.toString(2)//转为2进制
x.toString(8)//转为8进制
x.toString(16)//转为16进制
```

parseInt方法将其他进制数字转为十进制

```js
var x = "110"//这是一个二进制的字符串表示
parseInt(x, 2)//把这个字符串当做二进制， 转为十进制

var x = "70"//这是一个八进制的字符串表示
parseInt(x, 8)//把这个字符串当做八进制， 转为十进制

var x = "ff"//这是一个十六进制的字符串表示
parseInt(x, 16)//把这个字符串当做十六进制， 转为十进制
```



parseInt方法是用于将字符串转为数字的方法。 接受两个参数。 第一个要转换的字符串，第二个是可选的， 如果没有值， 默认是10进制； 如果有值， 就是以该值为转换进制

## JSON格式

所有对象的属性名必须是字符串，必须加引号

对象的最后一个元素后面不允许加逗号

js中对象：

```js
var obj = {
    name:"张三",
}
```

json中的对象：

```json
{
    “name“：”张三”
}
```

例：

```json
[
    {
        "name":"手机",
        "price":"￥1999",
        "color":"red"
    },
    {
        "name":"电脑",
        "price":"￥3998",
        "color":"green"
    }
]
```



## Date对象

Date对象用来处理日期和时间

### 创建日期对象

```js
var date = new Date();//使用构造函数创建一个当前时间的对象
var date = new Date("2017-03-22");//创建一个指定时间的日期对象
var date = new Date("2017-03-22 00:52:34");//创建一个指定时间的日期对象
var date = new Date(2017, 2, 22, 0, 52, 34);
var date = new Date(1523199394644);//参数：毫秒值
```

结果：Tue Jul 30 2019 21:26:56 GMT+0800 (中国标准时间)

类型使用自 CTU（Coordinated Universal Time，国际协调时间）1970 年 1 月 1 日午夜（零时）开始经过的毫秒数来保存日期。Date 类型保存的日期能够精确到 1970 年 1 月 1 日之前或之后的 285616  年。

Date构造函数当不传递任何参数的时候，返回的是当前时间。当传入参数的时候，获取的是传进去的时间

### 获取时间

使用日期对象的方法获取日期的指定部分

```js
getMilliseconds();//获取毫秒值
getSeconds();//获取秒
getMinutes();//获取分钟
getHours();//获取小时
getDay();//获取星期，0-6    0：星期天
getDate();//获取日，即当月的第几天
getMonth();//返回月份，注意从0开始计算，这个地方坑爹，0-11
getFullYear();//返回4位的年份  如 2016
```

案例：将日期格式化成字符串

```js
function formatDate(date) {
    var d = new Date(date),
      month = '' + (d.getMonth() + 1),
      day = '' + d.getDate(),
      year = d.getFullYear();
   
    if (month.length < 2) month = '0' + month;
    if (day.length < 2) day = '0' + day;
   
    return [year, month, day].join('-');
  }

  console.log(formatDate('Sun May 13,2016'));
```

案例：获取当前时间 年月日时分秒 （刷新）

```html
<div id="box"></div>
<script>
    function showNowTime(){
        var date = new Date();
        var year = date.getFullYear();  // 获取年
        var month = date.getMonth()+1; // 获取月 日期对象中使用0~11来表示1~12月
        var d = date.getDate(); // 获取日
        var day = date.getDay(); // 获取星期几
        var hour = date.getHours(); // 获取到时
        var minute = date.getMinutes(); // 获取分钟
        var second = date.getSeconds(); // 获取到秒数
        // var msecond = date.getMilliseconds(); // 获取到毫秒数
        // console.log(month);
        box.innerText = ("现在是北京时间："+year+"年"+month+"月"+d
        +"日。星期"+day+"。"+hour+"时"+minute+"分"+second+"秒");
    }
    setInterval(showNowTime,1000);
</script>
```

### 日期转为毫秒数(时间戳)

格林威治时间/格林尼治时间

```js
Date.parse("2015-08-24") // 获取1970年到设定时间的毫秒数
new Date().getTime()
+new Date();
```

案例：计算两个日期的时间差值

```js
function count(){
    var date1=new Date(2010,10,3);
    var date2=new Date(2017,9,24);
    var date=(date2.getTime()-date1.getTime())/(1000*60*60*24);/*不用考虑闰年否*/
    alert("相差"+date+"天");
}
```

案例：日期差值; 

```js
var date1 = new Date('2013/04/02 18:00')
var date2 = new Date('2013/04/02 19:22:21')
var s1 = date1.getTime(),s2 = date2.getTime();
var total = (s2 - s1)/1000;
var day = parseInt(total / (24*60*60));//计算整数天数
var afterDay = total - day*24*60*60;//取得算出天数后剩余的秒数
var hour = parseInt(afterDay/(60*60));//计算整数小时数
var afterHour = total - day*24*60*60 - hour*60*60;//取得算出小时数后剩余的秒数
var min = parseInt(afterHour/60);//计算整数分
var afterMin = total - day*24*60*60 - hour*60*60 - min*60;//取得算出分后剩余的秒数
```

案例：显示中文时间

```js
var chr=["零","一","二","三","四","五","六","七","八","九","十"];
init();
function init() {
    var div=document.getElementsByTagName("div")[0];
    setInterval(animation,16,div);
}
function animation(elem) {
    var date=new Date();
    var year=getChrYear(date.getFullYear());
    var month=getNumberChr(date.getMonth()+1)+"月";
    var days=getNumberChr(date.getDate())+"日";
    var week="星期"+(date.getDay()===0 ? "日" : getNumberChr(date.getDay()));
    var hours=date.getHours();
    hours=(hours<12 ? "上午 " : "下午 ")+getNumberChr(hours<12 ? hours : hours-12)+"点";
    var minutes=getNumberChr(date.getMinutes(),true)+"分";
    var seconds=getNumberChr(date.getSeconds(),true)+"秒";

    elem.innerHTML=year+month+days+"&nbsp;&nbsp;"+week
        +"&nbsp;&nbsp;"+hours+minutes+seconds;
}

function getChrYear(year) {
    var a=Math.floor(year/1000);
    var b=Math.floor(year/100)%10;
    var c=Math.floor(year/10)%10;
    var d=year%10;
    if(a!==0) return chr[a]+chr[b]+chr[c]+chr[d]+"年";
    //            三位
    if(b!==0) return chr[b]+chr[c]+chr[d]+"年";
    //           两位
    if(c!==0) return chr[c]+chr[d]+"年";
    //           一位
    return chr[d]+"年";
}

function getNumberChr(num,zero) {
    num=Math.floor(num);
    if(num<10) return (zero ? chr[0] : "")+chr[num];
    if(num===10) return chr[num];
    if(num<20) return "十"+chr[num%10];
    if(num%10===0 && num<100) return chr[Math.floor(num/10)]+"十";
    if(num<100) return chr[Math.floor(num/10)]+"十"+chr[num%10];
    if(num>100) return "输入错误";
}
```

### 日期格式化

```js
date.toLocalString();//本地风格的日期格式
date.toLocaleDateString(); // 获取日期
date.toLocaleTimeString(); // 获取时间
```

### 设置时间

```js
date.setFullYear(2016) // 将日期对象中的年份设置为2016
date.setMonth(3) // 将日期对象中的月份设置为4月份
date.setDate(15) // 将日期对象中的日期设置为15号
date.setHours(12) // 将日期对象中的小时设置为12点
date.setMinutes(56) // 将日期对象中的分钟设置为56分
date.setSeconds(23) // 将日期对象中的描述设置为23秒
date.setTime(0) // 将日期对象设置为0的时间戳
```

