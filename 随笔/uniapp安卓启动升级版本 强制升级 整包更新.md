```js
<script>
	export default {
    data() {
      return {
        updateContent: '',
        newVersion: '',
        downloadUrl: ''
      }
    },
	onLaunch: function() {
	  //#ifdef APP-PLUS
      var that = this;
      uni.getSystemInfo({
        success: (res) => {
          that.searchVersion(res.platform)
        }
      });
	  //#endif
	},
	methods: {
      async searchVersion(platform) {
        var that = this;
        var checkVersionApiUrl = "检查更新地址https";
        uni.request({
          method: 'POST',
        	url: checkVersionApiUrl,
          dataType: "json",
          data:{},
          header: {
            "Content-Type": "application/json;charset=utf8"
          },
        	success: (res) => {
        		if (res.statusCode == 200 && res.data.code == 0) {
              that.downloadUrl = res.data.data.url;
              that.newVersion = parseInt(res.data.data.version);
              if (platform == "android") {
                that.AndroidCheckUpdate();
              }
        		}
        	}
        })
      },
      async AndroidCheckUpdate() {
        let _this = this;
        let version = 8;
        this.updateContent = '升级版本'
        let hasNewVersion = _this.newVersion > version
        if (hasNewVersion) {
          //强制更新
          plus.nativeUI.confirm("您有新的APP版本需要更新",
              function(e) {
                if (e.index == 0) {
                  //调用封装方法判断用户网络环境
                  if (plus.networkinfo.getCurrentType() != 3) {
                    plus.nativeUI.confirm("检测到您目前非Wifi连接，是否继续更新", function(e) {
                      if (e.index == 0) {
                        //直接下载安装包
                        _this.downApk();
                      } else {
                        //安卓APP应用强制退出
                        plus.runtime.quit();
                      }
                    }, "", )["取消", "确定"]
                    return;
                  }
                  _this.downApk();
                } else {
                  plus.runtime.quit();
                }
              }, _this.updateContent, ["立即去", "退出"]);
        }

      },

      downApk() {
        let that = this
        let task = plus.downloader.createDownload(that.downloadUrl, {}, function(d, status) {
          if (status == 200) {
            plus.runtime.install(plus.io.convertLocalFileSystemURL(
                d.filename
                ), {}, {},
                function(error) {
                  console.log(error)
                  uni.showToast({
                    title: '错误',
                    mask: false,
                    duration: 1500
                  });
                })

          } else {
            plus.nativeUI.alert("下载错误！");
            plus.nativeUI.closeWaiting();
          }
          plus.nativeUI.closeWaiting();
        })
        //监听下载事件
        task.addEventListener("statechanged", function(download, status) {
          switch (download.state) {
            case 2:
              showLoading.setTitle("已连接到服务器");
              break;
            case 3:
              //进度条百分比 totalSize为总量，baifen为当前下载的百分比
              prg = parseInt((parseFloat(task.downloadedSize) / parseFloat(task.totalSize)) *
                  100);
              showLoading.setTitle("  正在下载" + prg + "%  ");
              break;
            case 4:
              // mui.toast("下载完成");
              plus.nativeUI.closeWaiting();
              break;
          }
        });
        task.start();
        var prg = 0;
        var showLoading = plus.nativeUI.showWaiting("正在下载");
      },
    }
	};
</script>

```

