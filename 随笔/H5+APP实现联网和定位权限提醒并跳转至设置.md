H5+APP的项目需要用户联网并且定位，没有开启相关权限将导致APP无法正常使用。故在程序打开是需要检测用户的手机是否开启相关权限，没有开启将提醒用户开启。

# HTML5 网络状态获取

```
常量：
CONNECTION_UNKNOW: 网络状态常量，表示当前设备网络状态未知，固定值为0。
CONNECTION_NONE: 网络状态常量，当前设备网络未连接网络，固定值为1。
CONNECTION_ETHERNET: 网络状态常量，当前设备连接到有线网络，固定值为2。
CONNECTION_WIFI: 网络状态常量，当前设备连接到无线WIFI网络，固定值为3。
CONNECTION_CELL2G: 网络状态常量，当前设备连接到蜂窝移动2G网络，固定值为4。
CONNECTION_CELL3G: 网络状态常量，当前设备连接到蜂窝移动3G网络，固定值为5。
CONNECTION_CELL4G: 网络状态常量，当前设备连接到蜂窝移动4G网络，固定值为6。
判断网络情况
var connectionStatus = plus.networkinfo.getCurrentType();
if(connectionStatus == 0 || connectionStatus == 1){
    mui.toast('无法连接网络');
}else if(connectionStatus == 3){
    mui.toast('使用wifi');
}else{
    ........
}
```

# HTML5 定位获取

```
function getLocation(){ 
  if (navigator.geolocation){ 
    navigator.geolocation.getCurrentPosition(showPosition,showError); 
  }else{ 
    alert("浏览器不支持地理定位。"); 
  } 
}
```

# 联网提醒设置

```
//获取当前网络类型
	　　var nt = plus.networkinfo.getCurrentType();
		if (nt == plus.networkinfo.CONNECTION_NONE) {
			if(localStorage.getItem('netStatus') == null || localStorage.getItem('netStatus') == 1){
				var btnArray = ['取消', '打开网络'];
				localStorage.setItem('netStatus','2');
				mui.confirm('您需要打开网络，才可以使用【云师傅】。请到设置->无线局域网(或蜂窝移动网络)中开启。','网络已关闭', btnArray, function(e) {
				  if (e.index == 1) { 
					if(mui.os.ios) {
						var UIApplication = plus.ios.import("UIApplication");
						var NSURL = plus.ios.import("NSURL");
						var setting = NSURL.URLWithString("app-settings:");
						var application = UIApplication.sharedApplication();
						application.openURL(setting);
						plus.ios.deleteObject(setting);
						plus.ios.deleteObject(application);
						localStorage.setItem('netStatus',0);
 						//判断手机定位功能是否开启
						app.isOpenPositionin();
					}else{
						var main = plus.android.runtimeMainActivity();
						var Intent = plus.android.importClass("android.content.Intent");
						var mIntent = new Intent('android.settings.WIFI_SETTINGS');
						main.startActivity(mIntent);
						localStorage.setItem('netStatus',0);
						//判断手机定位功能是否开启
						app.isOpenPositionin();
					}
					} else {
						localStorage.setItem('netStatus','1');
						owner.doExitApp();            
					}   
				}) 
			}
		}else{
			localStorage.setItem('netStatus','0');
			//判断手机定位功能是否开启
			app.isOpenPositionin();
		}
```

# 定位提醒设置

```
plus.geolocation.getCurrentPosition(function(position){
},function(e){
    if(localStorage.getItem('netStatus') == 0){
        localStorage.setItem('netStatus','3');
        var btnArray = ['取消', '设置'];
        mui.confirm('您需要打开定位权限，才可以使用【云师傅】。该位置信息用于在管理后台记录您的工作轨迹。请到设置->隐私->定位服务中开启。','定位服务已关闭', btnArray, function(e) {
            if (e.index == 1) { 
				if(mui.os.ios) {
                     var UIApplication = plus.ios.import("UIApplication");
                     var NSURL = plus.ios.import("NSURL");
                     var setting = NSURL.URLWithString("app-settings:");
                     var application = UIApplication.sharedApplication();
                     application.openURL(setting);
                     plus.ios.deleteObject(setting);
                     plus.ios.deleteObject(application);
                    }else{
                        var main = plus.android.runtimeMainActivity();
                        var Intent = plus.android.importClass("android.content.Intent");
                        var mIntent = new Intent('android.settings.LOCATION_SOURCE_SETTINGS');
                        main.startActivity(mIntent);
                    }
					} else {
                localStorage.setItem('netStatus',0);
                owner.doExitApp();            
								  }              
				})
			}
},{
			provider:'baidu',
			geocode:'true'
})
```