## 一、下载并安装*VMware虚拟化软件*

百度搜索关键词

![img](https://img-blog.csdnimg.cn/44aa2e1ec2564fd8bd55645d9f0de8a8.png)

 安装步骤：傻瓜式安装（鼠标点点点，这里不做截屏演示）

## 二、安装秘钥

1、百度搜索关键词VMware秘钥，秘钥不一定都有效，一个一个试总有一个可以的

![img](https://img-blog.csdnimg.cn/bbef4f51d8554c21b2c261b36e5f4c53.png)

##  三、安装VMware虚拟机

1、双击打开应用，选择[新建虚拟机](https://so.csdn.net/so/search?q=新建虚拟机&spm=1001.2101.3001.7020)

![img](https://img-blog.csdnimg.cn/1e4656e0a242416b9aab1a7089de7866.png)

2、 可以选择自定义也可以选择典型，此处演示自定义 

![img](https://img-blog.csdnimg.cn/25b9009cd2d1439c8e0ce445d55e7ced.png)

3、此处一般默认，点击下一步

![img](https://img-blog.csdnimg.cn/dbf32ba830d74b4f98a3ceacc1b320e0.png)

4、此处选择稍后安装操作系统

![img](https://img-blog.csdnimg.cn/4c200a674a8143e7857f986f0b73af67.png)

5、选择客户机操作系统，此处以Linux操作系统CentOS 7为例

![img](https://img-blog.csdnimg.cn/1ba4658fa5ae42cead98949f66b70060.png)

6、自定义虚拟机名称和虚拟机位置，位置默认在C盘，建议存放在D盘

 ![img](https://img-blog.csdnimg.cn/3e4ef0e296e54b2991992dfa124940f0.png)

 7、视[电脑配置](https://so.csdn.net/so/search?q=电脑配置&spm=1001.2101.3001.7020)情况自定义处理器CPU核数、线程数，单击下一步

![img](https://img-blog.csdnimg.cn/0d2d504b045346c6b4d59e41dece4014.png)

8、 视电脑配置情况自定义虚拟机内存，单击下一步

![img](https://img-blog.csdnimg.cn/b884b4c59b7b49ff941b60aae92308f1.png)

9、配置网络类型（如果想要连接外网，选用桥接或者NAT，具体之间有何区别请看图片中的解释），单击下一步

![img](https://img-blog.csdnimg.cn/ac31b426d8ea4d4183874b3c452498da.png)

10、 选择IO控制类型，一般情况选择默认，如果你的磁盘性能足够优越，可选择准虚拟化，单击下一步

![img](https://img-blog.csdnimg.cn/4a6dff481adb4d669e719abc76cb56a5.png)

 11、选择磁盘类型，推荐SCSI，也可选择其他，但是如果选择其他后面可能出现无法读取镜像文件的情况

![img](https://img-blog.csdnimg.cn/14aec8fe149940cdaa1427fd64a7b818.png)

12、选择磁盘创建，默认下一步

![img](https://img-blog.csdnimg.cn/207c1eb2edb24f219b765eb5dfc745e0.png)

13、 配置磁盘容量，视情况配置容量，建议不低于8GB，其他如下图所示，单击下一步

 ![img](https://img-blog.csdnimg.cn/678e30fa4f3d485096408ecdcdfc12fa.png)

![img](https://img-blog.csdnimg.cn/6b5e6aa53b424d959a19fe965103d847.png)

14、自定义硬件，包括但不限于增删网卡、声卡，选择ISO镜像文件，开启CPU虚拟化等，配置完成后点击关闭，点击完成

![img](https://img-blog.csdnimg.cn/0d667e9df6734607a1830c6e7629ebe2.png)

![img](https://img-blog.csdnimg.cn/23be5082264c4867ae8679f10855a212.png)

## ![img](https://img-blog.csdnimg.cn/9e0b594c53214263bd9033cdd29c910b.png)![img](https://img-blog.csdnimg.cn/e69c4833cf0949a78f1fffd320281ce1.png)四、配置虚拟机

1、开启虚拟机

![img](https://img-blog.csdnimg.cn/7e7b9a95a5eb49b7aab0ebc240ee7373.png)

2、 将鼠标点进去，按上键选择第一个，然后回车加载镜像文件，也可以直接回车

![img](https://img-blog.csdnimg.cn/db21eb12fb0548cda08c78b16773806e.png)![img](https://img-blog.csdnimg.cn/ae8474f5f9a4458da5f88f332424abe0.png)

3、 在此界面配置语言信息，喜欢用英语的直接点击下一步，不喜欢英语的可选其他，此处演示中文

![img](https://img-blog.csdnimg.cn/c9b1a650e2d54f31b6599048929ff193.png)![img](https://img-blog.csdnimg.cn/ea748b5c3c3440ca86bbd1c072722a6c.png)

4、 来到这个界面可操作的东西较多

![img](https://img-blog.csdnimg.cn/b2a8f183ea9447efb1018466de994e5b.png)

![img](https://img-blog.csdnimg.cn/b3880fc070d3429f8f7bfac45a5427cb.png)

5、 如果对镜像的安装有特殊要求，开在此处选择需要安装的环境，此处默认最小化安装，点击开始安装

 ![img](https://img-blog.csdnimg.cn/bb729a500b524fc6bc678e95eb10f34b.png)

![img](https://img-blog.csdnimg.cn/2fa01dcc36b74d6685883bc1b77dc6ca.png)

![img](https://img-blog.csdnimg.cn/6b55f2af6a04473695c87f96bec6b748.png)

6、 此处设置root密码

![img](https://img-blog.csdnimg.cn/1eadbd7ab0c34ec6a7cf6fbde58d9216.png)

7、如果root密码过于简单，需要点击两次完成

![img](https://img-blog.csdnimg.cn/add6a8ae83b64e7f83120bc983070872.png)

8、 等待安装完成，安装时间比较漫长，安装时间与电脑自身的配置有关（比如电脑CPU性能、内存读写速率，磁盘IOPS）以及镜像文件大小和镜像文件内部启动文件的配置情况有关，一般等待时间在2-15min不等，安装完成后点击重启，重启虚拟机

![img](https://img-blog.csdnimg.cn/19752c99a06d4a7d9a74637e03ddde06.png)

9、重启完成后，输入用户名密码

![img](https://img-blog.csdnimg.cn/71244c70327b4a7f869865fcf4f488c2.png)

10、 如果出现无法访问外网的情况，如下

![img](https://img-blog.csdnimg.cn/147cb4686863416f8ece285298e9fd0b.png)

## **五、 如果出现这种情况，可能有如下原因：**

①、虚拟机网卡配置有误

②、虚拟机的部分服务未启用

③、电脑防火墙未关闭，流量遭到拦截

④、虚拟网卡被禁用

针对如上第①情况，解决办法如下：

1、点击编辑，打开虚拟网络编辑器，查看IP信息

![img](https://img-blog.csdnimg.cn/2e2ebecb098a44458ec8709ed6456345.png)

 ![img](https://img-blog.csdnimg.cn/42839bfe2d474d20aef18fbcb1064ff8.png)

 ![img](https://img-blog.csdnimg.cn/aa1a702353de4e63809f36891fd905d3.png)

 2、明确虚拟网卡的IP地址段、掩码、网关后进入相关目录查看网卡配置文件，如下

 cd /etc/sysconfig/network-scripts/

ls

cat ifcfg-ens33 

![img](https://img-blog.csdnimg.cn/107ba99c5aae40d4a87239188ddcf74e.png)

 ![img](https://img-blog.csdnimg.cn/953270e258a0467382940d175d4b6131.png)

3、 若网卡配置文件有问题，则编辑网卡配置文件

vi ifcfg-ens33

TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"        #此处若为静态则需要修改为static
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="61b84719-ad0d-4ac1-bdb2-9c898ff3586f"
DEVICE="ens33"
ONBOOT="yes"          #此处若为no则修改为yes
IPADDR="192.168.67.99"   #配置正确的IP地址
PREFIX="24"            #配置正确的掩码
GATEWAY="192.168.67.2"  #配置正确的网关
DNS1="8.8.8.8"           #配置正确的DNS
DNS2="114.114.114.114"
IPV6_PRIVACY="no"

然后wq保存退出

4、重启网卡，命令如下

systemctl restart network或service network restart

![img](https://img-blog.csdnimg.cn/ec4b4d8181cb4a24bc17b057c051e763.png)

5、检测是否能够正常连接外网

![img](https://img-blog.csdnimg.cn/1795b06cf4254965a81ff0a6f3059b88.png)