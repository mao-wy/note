利用adb无线连接android手机进行调试 无需获得root权限
要想使用无线调试有两个必须条件

1.手机和我们的电脑要处于同一网络，就是两个设备之间的ip地址能够连通。

2.安装了adb，作为android开发者都应该知道它的路径是在sdk下的 platform-tools的文件夹里面，当然你也可以单独下adb。 有了上面两个条件，下面我们来说下如何实现无线连接调试我们的应用程序。

方法(方便简单适用几乎所有设备)
1.首先把我们的手机连接到电脑上。
2.在命令行里cd到我们的sdk下的adb.exe的路径找到我们的adb命令输入 ,输入adb devices查看我们连接的设备

3.使用adb tcpip 8888 设置端口号，5555为默认端口号，也可以设置其它端口号，端口号为需要为4位数

4.拔掉我们的设备，开始无线连接 adb connect
使用adb connect 192.168.1.39:8888， 192.168.1.39为我们手机的ip地址， 其中8888是我们自己设的端口号，这个端口号要和adb tcpip 设置的端口号保持一样，如果我们没有自己设置端口号，直接adb connect192.168.1.39就行了，默认使用5555。 连接成功提示

取消连接就是 adb disconnect
adb disconnect 192.168.1.39:8888

利用adb无线连接android手机进行调式,无需获得root权限 可参考原文
原文：https://blog.csdn.net/lnking1992/article/details/53465183





adb相关



查看连接

adb devices



通过ip连接默认端口号5555，首先设备一定开启了远程端口

adb connect 192.168.2.93

adb connect 10.0.1.18

90之后代表大屏



获取当前显示的app包名及当前打开的页面名称

adb shell 

再输入

dumpsys window | grep mFocusedApp



adb shell dumpsys window | findstr mCurrentFocus



此命令可以过滤出该应用的进程号PID

adb shell "ps|grep cn.yishikj.smallScreen"



根据PID抓包

adb shell "logcat | grep 1064"



如果有别的设备连接必须断开连接后才能连接

断开全部

adb disconnect 

adb disconnect 192.168.2.93



远程连接

scrcpy



选择设备远程

scrcpy -s 设备id



远程shell

adb -s 10.0.1.90:5555 shell



内核版本查看

adb shell getprop ro.product.cpu.abi

arm64-v8a



查看安装所有app

adb shell pm list package -3



回到首页

adb shell am start -n com.android.launcher3/com.android.launcher3.Launcher



打开守护进程APP

adb shell am start -W -S com.lztek.bootmaster.autoboot7/com.lztek.bootmaster.MainActivity t69



打开定时器APP

adb shell am start -W -S com.lztek.bootmaster.poweralarm7/com.lztek.bootmaster.MainActivity t557



打开大屏首页

adb -s 192.168.2.78:5555 shell am start -W -S cn.yishikj.bigScreen/cn.yishikj.m.MainActivity



有感小屏

adb shell am start -W -S 

cn.yishikj.smallScreen/io.dcloud.PandoraEntryActivity t64



查看安装所有app包含webview

adb shell "pm list C:\Users\Administrator>adb shell "pm list package |grep webview"

package:com.google.android.webview



打开800/1000起点屏

adb shell am start -W -S cn.yishikj.longRun/cn.yishikj.m.MainActivity



按包名查看安装的影响信版本 webview版本

adb shell "pm dump com.google.android.webview|grep version"



   versionCode=410410153 minSdk=21 targetSdk=30

   versionName=83.0.4103.101



安装应用

adb install -r xxx.apk

apk需要绝对路径，可以拖拽过来



远程查看日志

adb logcat -v time

帮助我们远程查看APP的系统日志



配合google进行远程调试在浏览器中打开

chrome://inspect/#devices

调试如果出错 验证是否能够打开 https://chrome-devtools-frontend.appspot.com/ 打不开需要fan墙访问





------------- monkey 命令相关------------- 



adb -s 192.168.2.78:5555 shell monkey -p cn.yishikj.bigScreen --throttle 500 --pct-touch 100 -v 1000



------------- monkey 命令相关------------- 



------------- adb 自动化------------- 

打开应用

adb -s 192.168.2.78:5555 shell am start -W -S cn.yishikj.bigScreen/io.dcloud.PandoraEntryActivity t64



打开Android原生打的包，cn.yishikj.bigScreen是ApplicationId

adb -s 192.168.2.78:5555 shell am start -W -S cn.yishikj.bigScreen/cn.yishikj.m.MainActivity



模拟点击

adb -s 192.168.2.78:5555 shell input tap 153 559



adb -s 192.168.2.78:5555 shell input text zhanghao 



rem 模拟等等一秒

ping 127.0.0.1 -n 1



保存到文件 修改为bat

------------- adb 自动化------------- 



------------- 清除密码------------------

adb shell

cd data/system

删除以下密码文件

rm gatekeeper.password.key

rm gatekeeper.pattern.key

rm locksettings.db

rm locksettings.db-shw

rm locksettings.db-wal

------------- 清除密码------------------



----- 开启远程端口 -------

安卓7.1中可以在设置中开启 不需要使用下方配置



\## 使用usb连接设备

adb devices

\## 设备已连接的情况下

切换adb服务使用对象为root用户

adb root 

adb remount 

拉取文件

adb pull system/build.prop .

\>> ...... 修改这个文件：

\>> 在文件的末尾加上：等同于 adb tcpip 5555

service.adb.tcp.port=5555

\>> 保存文件

adb push build.prop system/build.prop



\## 在 push build.prop 文件之后

adb shell

cd system/

ls -ll

// 此时会发现build.prop文件的权限为：-rw-rw-rw-

chmod 0644 build.prop

ls -ll

// 此时build.prop文件的权限就变为了：-rw-r--r--

exit

adb reboot

\## 等待开机



操作完上面的步骤之后，很多人会直接执行adb reboot来重启设备。

adb reboot

----- 开启远程端口结束 -------