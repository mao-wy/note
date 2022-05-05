```js
//监听底部中间菜单的事件  
plus.android.requestPermissions(['android.permission.CAMERA'], function(e){  
    if(e.deniedAlways.length>0){    //权限被永久拒绝  
        // 弹出提示框解释为何需要权限，引导用户打开设置页面开启  
        console.log('权限被永久拒绝'+e.deniedAlways.toString());
        // 创建扫码控件来调获取手机的相机权限
        var barcode = plus.barcode.create('barcode', [plus.barcode.QR], {
            top: '100px', //改为-9999px隐藏该页面
            left: '0',
            width: '100%',
            height: '500px',
            position: 'static'
        });
        plus.webview.currentWebview().append(barcode);
        //barcode.start();开始扫码识别（我们把这句代码注释了，因为我们不需要扫描任何东西）
    }
    if(e.deniedPresent.length>0){   //权限被临时拒绝  
        // 弹出提示框解释为何需要权限，可再次调用plus.android.requestPermissions申请权限  
        console.log('权限被临时拒绝'+e.deniedPresent.toString());  
    }  
    if(e.granted.length>0){ //权限被允许  
        console.log('权限被允许'+e.granted.toString());  
    }  
}, function(e){  
    console.log('Request Permissions error:'+JSON.stringify(e));  
});
```

