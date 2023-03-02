

WebSocket 属性

 ws.readyState （ 只读属性 readyState 表示连接状态）
 0:表示连接尚未建立。
 1:表示连接已建立，可以进行通信。
 2:表示连接正在进行关闭。
 3:表示连接已经关闭或者连接不能打开。
1
2
3
4
5
WebSocket方法

ws.send();    //使用连接发送数据
ws.close();    //关闭socket链接



```
let Socket = null
let setIntervalWesocketPush = null
const socketUrl = 'ws:127.0.0.1:8080/goLink/manager/'  // socket连接地址
import store from '@/store'
/**
 * 建立websocket连接
 * @param {string} url ws地址
 */
export const createSocket = () => {
  Socket && Socket.close()
  if (!Socket) {
    console.log('建立websocket连接')
    let userid = store.state.user.name;// 这里我是为了获取登录的用户信息
    if(userid){
      Socket = new WebSocket(socketUrl+userid)
      Socket.onopen = onopenWS
      Socket.onmessage = onmessageWS
      Socket.onerror = onerrorWS
      Socket.onclose = oncloseWS
    }else {
      this.$message.error("websocket连接失败，未获取到账户信息")
    }
  } else {
    console.log('websocket已连接')
  }
}
 
/**打开WS之后发送心跳 */
const onopenWS = () => {
  // sendPing()
}
 
/**连接失败重连 */
const onerrorWS = () => {
  Socket.close()
  clearInterval(setIntervalWesocketPush)
  console.log('连接失败重连中')
  if (Socket.readyState !== 3) {
    Socket = null
    createSocket(socketUrl)
  }
}
 
/**WS数据接收统一处理 */
const onmessageWS = e => {
  window.dispatchEvent(new CustomEvent('onmessageWS', {
    detail: {
      data: e.data
    }
  }))
}
 
/**
 * 发送数据但连接未建立时进行处理等待重发
 * @param {any} message 需要发送的数据
 */
const connecting = message => {
  setTimeout(() => {
    if (Socket.readyState === 0) {
      connecting(message)
    } else {
      Socket.send(JSON.stringify(message))
    }
  }, 1000)
}
 
/**
 * 发送数据
 * @param {any} message 需要发送的数据
 */
export const sendWSPush = message => {
  if (Socket !== null && Socket.readyState === 3) {
    Socket.close()
    createSocket(socketUrl)
  } else if (Socket.readyState === 1) {
    Socket.send(JSON.stringify(message))
  } else if (Socket.readyState === 0) {
    connecting(message)
  }
}
 
 
/**断开重连 */
const oncloseWS = () => {
  console.log('websocket已断开')
  Socket = null
  /*clearInterval(setIntervalWesocketPush)
  console.log('websocket已断开....正在尝试重连')
  if (Socket.readyState !== 2) {
    Socket = null
    createSocket()
  }*/
}
 
export const closeWs =() =>{
  Socket.close()
}
 
 
/**发送心跳
 * @param {number} time 心跳间隔毫秒 默认5000
 * @param {string} ping 心跳名称 默认字符串ping
 */
export const sendPing = (time = 5000, ping = 'ping') => {
  clearInterval(setIntervalWesocketPush)
  Socket.send(JSON.stringify({"event":"heart"}))
  setIntervalWesocketPush = setInterval(() => {
    Socket.send(JSON.stringify({"event":"heart"}))
  }, time)
}
```

在vue 添加方法

```
import { createSocket,sendWSPush,closeWs } from '../../../utils/websocket'
```

```
    wsConnect(){
        createSocket()
        this.getsocketData = (e) => {
          // 创建接收消息函数
          let data = e && e.detail.data
          if(data){
            console.log(data)
            let json = JSON.parse(data);
            if (json.event == 'open') {// 已经建立了连接
              this.dialogVisible = true
              let msg = {"event":'open_screen',"to_user":this.userId}
              sendWSPush(msg)
            }
          }
        }
        // 注册监听事件
        window.addEventListener('onmessageWS', this.getsocketData)
      },
      handleClose(){
        //想服务器发送断开连接的信息
        let msg = {"event":'stop_screen',"to_user":this.userId}
        sendWSPush(msg)
        closeWs()
        this.dialogVisible = false
        window.removeEventListener('onmessageWS',this.getsocketData)
      },
      screenSharing(userid){
        this.userId = userid
        this.wsConnect()
      },
```

 后端 使用的java进行处理

```
package io.renren.modules.xmkj.handler;
 
import cn.hutool.core.date.DateField;
import cn.hutool.core.date.DateUnit;
import cn.hutool.core.date.DateUtil;
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import io.renren.common.utils.SpringContextUtils;
import io.renren.modules.xmkj.dto.ResultDto;
import io.renren.modules.xmkj.entity.ScriptEntity;
import io.renren.modules.xmkj.entity.SqPhonesEntity;
import io.renren.modules.xmkj.entity.YyzsTaskLogsEntity;
import io.renren.modules.xmkj.entity.YyzsTaskScriptEntity;
import io.renren.modules.xmkj.service.ScriptService;
import io.renren.modules.xmkj.service.SqPhonesService;
import io.renren.modules.xmkj.service.YyzsTaskLogsService;
import io.renren.modules.xmkj.service.YyzsTaskScriptService;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.springframework.stereotype.Component;
 
import javax.websocket.*;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import java.io.IOException;
import java.util.Date;
import java.util.List;
import java.util.concurrent.ConcurrentHashMap;
 
/**
 * 自动化运营后台
 */
@ServerEndpoint(value ="/goLink/manager/{userId}") // 通过这个 spring boot 就可以知道你暴露出去的 ws 应用的路径，有点类似我们经常用的@RequestMapping。比如你的启动端口是8080，而这个注解的值是ws，那我们就可以通过 ws://127.0.0.1:8080/ws 来连接你的应用
@Component
@Slf4j
public class WebSocketManagerHandler {
    /**
     * 静态变量，用来记录当前在线连接数。应该把它设计成线程安全的。
     */
    private static int onlineCount = 0;
    /**
     * concurrent包的线程安全Set，用来存放每个客户端对应的MyWebSocket对象。
     */
    private static ConcurrentHashMap<String, WebSocketManagerHandler> webSocketMap = new ConcurrentHashMap<>();
    /**
     * 与某个客户端的连接会话，需要通过它来给客户端发送数据
     */
    private Session session;
    /**
     * 接收userId
     */
    private String userId = "";
    /**
     * 连接建立成功调用的方法
     */
    @OnOpen
    public void onOpen(Session session, @PathParam("userId") String userId) {
        this.session = session;
        this.userId = userId;
//        this.source = source;
        if(userId==null){
            log.error("无userId");
            serverClose();
        }else {
 
            //连接
            if (webSocketMap.containsKey(userId)) {
                webSocketMap.remove(userId);
                //加入set中
                webSocketMap.put(userId, this);
            } else {
                //加入set中
                webSocketMap.put(userId, this);
                //在线数加1
                addOnlineCount();
            }
 
            log.info("用户连接:" + userId + ",当前在线人数为:" + getOnlineCount());
 
            try {
                JSONObject json = new JSONObject();
                json.put("event","open");
                json.put("msg","ok");
                sendMessage(json.toJSONString());
            } catch (IOException e) {
                log.error("用户:" + userId + ",网络异常!!!!!!");
            }
        }
    }
 
    /**
     * 连接关闭调用的方法
     */
    @OnClose
    public void onClose() {
        if (webSocketMap.containsKey(userId)) {
            webSocketMap.remove(userId);
            //从set中删除
            subOnlineCount();
        }
        log.info("用户退出:" + userId + ",当前在线人数为:" + getOnlineCount());
    }
 
    /**
     * 主动断开连接
     */
    public void serverClose(){
        try {
            this.session.close();
            this.onClose();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
    /**
     * 收到客户端消息后调用的方法
     *
     * @param message 客户端发送过来的消息
     */
    @OnMessage
    public void onMessage(String message, Session session) {
        log.info("用户消息:" + userId + ",报文:" + message);
        //可以群发消息
        //消息保存到数据库、redis
        if (StringUtils.isNotBlank(message)) {
            try {
                //解析发送的报文
                JSONObject jsonObject = JSON.parseObject(message);
                switch (jsonObject.getString("event")){
                    case "heart":{
                        //检查当前的手机是否有执行的自动化任务
                        break;
                    }
                    case "open_screen":{
                        //检查当前的手机是否有执行的自动化任务
                        String to_user = jsonObject.getString("to_user");
                        ResultDto res = new ResultDto();
                        res.setEvent("open_screen");
                        res.setFromUser(userId);//来自服务器
                        WebSocketDHZSHandler.sendInfo(JSON.toJSONString(res),to_user);
                        break;
                    }
                    case "stop_screen":{
                        String to_user = jsonObject.getString("to_user");
                        ResultDto res = new ResultDto();
                        res.setEvent("stop_screen");
                        res.setFromUser(userId);//来自服务器
                        WebSocketDHZSHandler.sendInfo(JSON.toJSONString(res),to_user);
                        break;
                    }
                    default:
                        JSONObject jsonRes = new JSONObject();
                        jsonRes.put("fromuserId", "system");
                        jsonRes.put("event", "heart");
                        jsonRes.put("msg", "event无法处理");
                        sendMessage(jsonRes.toJSONString());
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
 
    /**
     * @param session
     * @param error
     */
    @OnError
    public void onError(Session session, Throwable error) {
        log.error("用户错误:" + this.userId + ",原因:" + error.getMessage());
        error.printStackTrace();
    }
 
    /**
     * 实现服务器主动推送
     */
    public void sendMessage(String message) throws IOException {
        this.session.getBasicRemote().sendText(message);
    }
 
 
    /**
     * 发送自定义消息
     */
    public static boolean sendInfo(String message, @PathParam("userId") String userId) throws IOException {
        log.info("发送消息到:" + userId + "，报文:" + message);
        boolean flag = false;
        if (StringUtils.isNotBlank(userId) && webSocketMap.containsKey(userId)) {
            webSocketMap.get(userId).sendMessage(message);
            return !flag;
        } else {
            log.error("用户" + userId + ",不在线！");
            return  flag;
        }
    }
 
    public static synchronized int getOnlineCount() {
        return onlineCount;
    }
 
    public static synchronized void addOnlineCount() {
        WebSocketManagerHandler.onlineCount++;
    }
 
    public static synchronized void subOnlineCount() {
        WebSocketManagerHandler.onlineCount--;
    }
}
```



websocket 掉线重连（心跳包）
websocket超时没有消息自动断开连接，应对措施：

这时候我们就需要知道服务端设置的超时时间是多少，在小于超时时间内发送心跳包，有2中方案，一种是客户端主动发送上行心跳包，另一种方案是服务端主动发送下行心跳包。下面主要讲一下客户端也就是前端如何实现心跳包：。

首先了解一下心跳包机制

心跳包之所以叫心跳包是因为：它像心跳一样每隔固定时间发一次，以此来告诉服务器，这个客户端还活着。事实上这是为了保持长连接，至于这个包的内容，是没有什么特别规定的，不过一般都是很小的包，或者只包含包头的一个空包。

在TCP的机制里面，本身是存在有心跳包的机制的，也就是TCP的选项：SO_KEEPALIVE。系统默认是设置的2小时的心跳频率。但是它检查不到机器断电、网线拔出、防火墙这些断线。而且逻辑层处理断线可能也不是那么好处理。一般，如果只是用于保活还是可以的。

心跳包一般来说都是在逻辑层发送空的echo包来实现的。下一个定时器，在一定时间间隔下发送一个空包给客户端，然后客户端反馈一个同样的空包回来，服务器如果在一定时间内收不到客户端发送过来的反馈包，那就只有认定说掉线了。

在长连接下，有可能很长一段时间都没有数据往来。理论上说，这个连接是一直保持连接的，但是实际情况中，如果中间节点出现什么故障是难以知道的。更要命的是，有的节点（防火墙）会自动把一定时间之内没有数据交互的连接给断掉。在这个时候，就需要我们的心跳包了，用于维持长连接，保活。

心跳检测步骤：

客户端每隔一个时间间隔发生一个探测包给服务器
客户端发包时启动一个超时定时器
服务器端接收到检测包，应该回应一个包
如果客户机收到服务器的应答包，则说明服务器正常，删除超时定时器
如果客户端的超时定时器超时，依然没有收到应答包，则说明服务器挂了
前端解决方案：

```
//心跳检测
var heartCheck = {
    timeout: 30000,        //30秒发一次心跳
    timeoutObj: null,
    serverTimeoutObj: null,
    reset: function(){
        clearTimeout(this.timeoutObj);
        clearTimeout(this.serverTimeoutObj);
        return this;
    },
    start: function(){
        var self = this;
        this.timeoutObj = setTimeout(function(){
            //这里发送一个心跳，后端收到后，返回一个心跳消息，
            //onmessage拿到返回的心跳就说明连接正常
            ws.send("ping");
            console.log("ping!")
            self.serverTimeoutObj = setTimeout(function(){//如果超过一定时间还没重置，说明后端主动断开了
                ws.close();     //如果onclose会执行reconnect，我们执行ws.close()就行了.如果直接执行reconnect 会触发onclose导致重连两次
            }, self.timeout)
        }, this.timeout)
    }
}

```

