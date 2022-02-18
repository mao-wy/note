```
<template>
    <div class="Tracker">
        <div id="face_panel" class="col-xs-6 col-sm-6 col-md-6" style="padding-left: 50px;height: 90%;">
            <span style="">请勿在逆光或较暗环境下拍摄</span>
            <div>
                <video id='video' :class="[videoisplay ? 'show-display' : 'no-display']"  :width="videoWidth" :height="videoHeight" style="" autoplay></video>
            </div>
            <canvas id='myCanvas' :class="[isdisplay ? 'show-display' : 'no-display']" style=""></canvas>
            <a id="flip_btn" href="javascript:;" style="position: absolute;top: 2%;right: 5%;color: #0d71fe;">翻转</a>

            <div class="descerningDiv" style="background-image: linear-gradient(to bottom, #7676a799, #0000ff6e, blue);box-shadow: 0px -20px 15px #7676a799;">
                <span id="discern_flag" style="font-size: 30px;color: white">认证中。。。</span>
            </div>
            <!-- <div class="successDiv" :class="[isdisplay ? 'show-display' : 'no-display']"
                style="background-image: linear-gradient(#98d0b329, #03c15e82, #03C15E);box-shadow: 0px -20px 15px #98d0b329;">
                <span id="success_flag" style="font-size: 30px;color: white">认证成功</span>
            </div> -->
            <!-- <div class="errorDiv" style="background-image: linear-gradient(#f1797914, #ff505094, #FF5050);box-shadow: 0px -20px 15px #f1797914;">
                <span id="error_flag" style="font-size: 30px;color: white">认证失败</span>
            </div> -->
        </div>
    </div>
</template>

<script>
import { defineComponent, ref, reactive, onMounted } from 'vue'
import { ElMessage } from 'element-plus'
export default defineComponent({
    name:'Tracker',
    setup (props,context) {
        let isdisplay = ref(true),
            videoisplay = ref(true),
            ctx = ref("\/"),
            prefix = ctx.value + "core/content",
            taskContentId = ref(436),
            timeOutIndex = ref(),
            intervalIndex = ref(),
            lock = ref(true),
            isOpenMedia = ref(true),
            videoWidth = ref(600),
            videoHeight = ref(400),
            _layer_index = ref(),
            _nv = ref(null),
            front = ref(false),
            //访问用户媒体设备的兼容方法
            getUserMedia = (constraints, success, error) => {
                console.log('访问用户媒体设备的兼容方法')
                console.log(_nv,constraints)
                if (_nv.getUserMedia) {
                    //最新的标准API
                    console.log('最新的标准API')
                    _nv.getUserMedia(constraints).then(success).catch(error);
                } else if (_nv.webkitGetUserMedia) {
                    //webkit核心浏览器
                    console.log('webkit核心浏览器')
                    _nv.webkitGetUserMedia(constraints, success, error)
                } else if (_nv.mozGetUserMedia) {
                    //firfox浏览器
                    _nv.mozGetUserMedia(constraints, success, error);
                } else if (_nv.getUserMedia) {
                    //旧版API
                    _nv.getUserMedia(constraints, success, error);
                }
            },
            //调取成功
            success = (stream) => {
                isOpenMedia.value = true;
                ElMessage({message:'访问用户媒体设备成功',customClass:'my-message'})
                console.log('访问用户媒体设备成功')
                var video = document.getElementById('video');
                var _stream = stream;
                //兼容webkit核心浏览器
                let CompatibleURL = window.URL || window.webkitURL;
                //将视频流设置为video元素的源
                // video.src = CompatibleURL.createObjectURL(stream);
                if ("srcObject" in video) {
                    video.srcObject = stream;
                } else {
                    // 防止在新的浏览器里使用它，因为它已经不再支持了
                    video.src = window.URL.createObjectURL(stream);
                }
                video.play();
                console.log('222222222')
                document.getElementById("#flip_btn").bind("click").click(function () {
                    front = !front.value;
                    document.getElementById("#flip_btn").css("color", front.value ? "white" : "#435382");
                    var track = _stream.getTracks()[0];
                    track.stop();
                    initMedia();
                });
            },
            //调取失败
            error = (error) => {
                // isOpenMedia = false;
                ElMessage({message:'访问用户媒体设备失败',customClass:'my-message'})
                console.log('访问用户媒体设备失败')
                // $.modal.alertWarning("访问用户媒体设备失败" + error.name + ":" + error.message);
            },
            initCanvas = () => {
                var c = document.getElementById("myCanvas");
                var cxtFace = c.getContext("2d");
                cxtFace.beginPath();
                cxtFace.arc(100, 80, 100, 0, Math.PI * 2, false);
                cxtFace.stroke();
                //填充背景色
                /*cxtFace.fillRect(0,0,1170,880);
                cxtFace.clearRect(300,180,520,520);*/
            },
            //点击事件
            navClick = () => {
                //切换认证模式
                document.querySelector(".nav_click").bind("click", function () {
                    // $(this).siblings().css("background-color", "white");
                    // $(this).siblings().css("color", "#358FFF");
                    // $(this).css("background-color", "#358FFF");
                    // $(this).css("color", "white");
                    // var _type = $(this).attr("data-type");
        //             if (_type == 2) {
        //                 //关闭视频取帧
        //                 clearInterval(intervalIndex);
        //                 clearTimeout(timeOutIndex);
        //                 lock = true;
        //                 $(".sub_btn").css("display", "none");
        //                 $("#face_panel").css("display", "none");
        //                 $("#card_panel").css("display", "block");
        //                 $("#myCanvas").css("display", "none");
        //                 $(".successDiv").css("display", "none");
        //                 $(".descerningDiv").css("display", "none");
        //                 $(".errorDiv").css("display", "none");
        //                 $("#video").css("display", "none");
        //             } else {
        //                 //循环取帧
        //                 loopFrame();
        // //        $(".sub_btn").css("display", "block");
        //                 $("#face_panel").css("display", "block");
        //                 $("#card_panel").css("display", "none");
        //                 $("#video").css("display", "block");
        //             }
        //         });
        //         //重新认证
        //         $(".sub_btn").bind("click", function () {
        // //      lock = true;
        //             $("#myCanvas").css("display", "none");
        //             $(".successDiv").css("display", "none");
        //             $(".descerningDiv").css("display", "none");
        //             $(".errorDiv").css("display", "none");
        //             $("#video").css("display", "block");
        //      loopFrame();

                });
            },
            //取帧
            faceAction = () => {
                console.log('555555555')
                isdisplay.value = true
                videoisplay.value = true
                // document.querySelector(".successDiv").css("display", "none");
                // document.querySelector(".descerningDiv").css("display", "none");
                // document.querySelector(".errorDiv").css("display", "none");
                // document.querySelector("#video").css("display", "block");
                lock = false;
                clearTimeout(timeOutIndex);
                timeOutIndex = setTimeout(function () {
                    var video = document.getElementById("video");
                    console.log(video)
                    var canvas = document.getElementById("myCanvas");
                    console.log(canvas)
                    var context = canvas.getContext('2d');
                    var vw = canvas.width;
                    var vh = canvas.height;
                    console.log(vw,vh,'vhvhvhvhvh')
                    context.beginPath();
                    context.drawImage(video, 0, 0, vw,vh);
                    var base64File = canvas.toDataURL();
                    var imgData = base64File.split("base64,")[1];
                    var formData = new FormData();
                    formData.append("imgData", imgData);
                    formData.append("taskContentId", taskContentId);
                    console.log(formData)
        //             $.ajax({
        //                 type: "post",
        //                 url: prefix + "/faceDiscern",
        //                 data: formData,
        //                 processData: false,
        //                 contentType: false,
        //                 beforeSend: function () {
        //                     $("#myCanvas").css("display", "block");
        //                     $(".successDiv").css("display", "none");
        //                     $(".descerningDiv").css("display", "block");
        //                     $(".errorDiv").css("display", "none");
        //                     $("#video").css("display", "none");
        //                     //清空stu_refresh
        //                 },
        //                 success: function (re) {
        //                     //成功检录一次后暂停，开启方式：1重新检录，2c++通知，3完成成绩测试
        //                     if (re.code != 0) {//当前组检录完毕
        //                         $(".successDiv").css("display", "none");
        //                         $(".descerningDiv").css("display", "none");
        //                         $(".errorDiv").css("display", "block");
        //                         $("#error_flag").text(re.msg);
        //                     }
        //                 },
        //                 error: function (error) {
        // //          lock = false;
        //                     $.modal.alertWarning(error);
        //                 }
        //             });
                }, 1000);
            },
            //定时取帧
            loopFrame = () => {
                clearInterval(intervalIndex);
                console.log(lock, isOpenMedia,'3333333')
                intervalIndex = setInterval(() => {
                    console.log(lock, isOpenMedia,'3333333')
                    if (lock.value && isOpenMedia.value) {
                        console.log('55555555555555')
                        faceAction();
                    }
                }, 3000);
            },
            //识别结果处理
            reViewStu = (data) => {
                var parseMsg = $.parseJSON(data);
                var result = parseMsg.result;
                var _msg = parseMsg.msg;
                if (result == 0) {//刷新数据
                    doFresh(parseMsg.taskContentId, parseMsg.groupId);
                    hasScore(parseMsg.groupId);
                } else if (result == 1) {//失败，继续取帧
                    $(".successDiv").css("display", "none");
                    $(".descerningDiv").css("display", "none");
                    $(".errorDiv").css("display", "block");
                    $("#error_flag").text(_msg);
                    lock = true;
                } else if (result == 2) {//测试位成功开启，继续取帧
                    doFresh(parseMsg.taskContentId, parseMsg.groupId);
                    //检测是否已经测出成绩
                    hasScore(parseMsg.groupId);
        //      lock = true;
                } else if (result == 3) {//测试位开启失败
                    $("#error_flag").text(_msg);
                } else if (result == 4) {
                    //检测是否已经测出成绩
                    hasScore(parseMsg.groupId);
        //      lock = true;
                } else if (result == 5) {//测试达到上限后置空测试位并刷新多组页面
                    $.get(prefix + "/resetLocation/taskContentId/" + parseMsg.taskContentId + "/groupId/" + parseMsg.groupId, function (re) {
                        if (re.code == 0) {//开启取帧
                            lock = true;
                        }
                    });
                }
            },
            //已测出成绩再次检录成功，弹出询问是否重测
            hasScore = (groupId) => {
                //检测是否已经测出成绩
                // $.get(prefix + "/getHasScore/" + groupId, function (re) {
                //     if (re.code == 0) {//已经测出成绩，弹出询问框
                //         layer.open({
                //             content: '您已测试，重新检录并测试会覆盖已测成绩，是否确认？',
                //             btn: ['确认', '取消'],
                //             shade: 0.3,
                //             shadeClose: false,
                //             closeBtn: 0,
                //             yes: function (index, layero) {
                //                 reTest(groupId);
                //                 layer.close(index);
                //             },
                //             btn2: function (index, layero) {
                //                 reLocation(groupId);
                //             }
                //         });
                //     } else {//无成绩记录
                //         lock = true;
                //     }
                // });
            },
            //重测
            reTest = (groupId) => {
                // $.get(prefix + "/reTest/" + groupId, function (re) {
                //     if (re.code == 0) {
                //         $.modal.msgSuccess("操作成功！");
                //         lock = true;
                //     } else {
                //         $.modal.alertError("操作失败！请稍后再试！");
                //     }
                // });
            },

            //重置跑道信息
            reLocation = (groupId) => {
                // $.get(prefix + "/reLocation/" + groupId, function (re) {
                //     if (re.code == 0) {
                //         lock = true;
                //     } else {
                //         $.modal.alertError("操作失败！请稍后再试！");
                //     }
                // });
            },
            doFresh = (taskContentId, groupId) => {
                document.querySelector(".successDiv").css("display", "block");
                document.querySelector(".descerningDiv").css("display", "none");
                document.querySelector(".errorDiv").css("display", "none");
                let url = prefix + "/faceRefresh/" + taskContentId + "/" + groupId;
        //         $.get(url, function (fragment) {
        //             $("#stu_refresh").replaceWith(fragment);
        // //      autoPage();
        //         });
            },
            initMedia = () => {
                var constraints = {audio: false, video: {facingMode: (front.value ? "user" : "environment")}};
                if (_nv.getUserMedia || _nv.webkitGetUserMedia || _nv.mozGetUserMedia || _nv.msGetUserMedia) {
                    //调用用户媒体设备, 访问摄像头
                    getUserMedia(constraints, success, error);
                    console.log('1111111111')
                } else {
                    isOpenMedia = false;
                    console.log('============')

                    // $.modal.alertError("不支持访问用户媒体！");
                }

            }
        onMounted(() => {
            
            // 老的浏览器可能根本没有实现 mediaDevices，所以我们可以先设置一个空的对象
            if (navigator.mediaDevices === undefined) {
                navigator.mediaDevices = {};
            }
            // 一些浏览器部分支持 mediaDevices。我们不能直接给对象设置 getUserMedia
            // 因为这样可能会覆盖已有的属性。这里我们只会在没有getUserMedia属性的时候添加它。
            if (navigator.mediaDevices.getUserMedia === undefined) {
                navigator.mediaDevices.getUserMedia = function (constraints) {
                    // 首先，如果有getUserMedia的话，就获得它
                    var getUserMedia = navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
                    // 一些浏览器根本没实现它 - 那么就返回一个error到promise的reject来保持一个统一的接口
                    if (!getUserMedia) {
                        console.log('一些浏览器根本没实现它 - 那么就返回一个error到promise的reject来保持一个统一的接口')
                        return Promise.reject(new Error('getUserMedia is not implemented in this browser'));
                    }
                    console.log('否则，为老的navigator.getUserMedia方法包裹一个Promise')
                    // 否则，为老的navigator.getUserMedia方法包裹一个Promise
                    return new Promise(function (resolve, reject) {
                        getUserMedia.call(navigator, constraints, resolve, reject);
                    });
                }
            }
            front = true;//true前置，false后置
            //初始化，调用摄像头
            _nv = navigator.mediaDevices != undefined ? navigator.mediaDevices : navigator;
            
            initMedia()
            var video = document.getElementById("video");
            var vwidth = video.videoWidth
            var vheight = video.videoHeight
            videoWidth.value = 1560 * document.documentElement.clientWidth / 2560
            videoHeight.value = 1230 * document.documentElement.clientWidth / 2560
            window.addEventListener('resize', ()=> {
                videoWidth.value = 1560 * document.documentElement.clientWidth / 2560
                videoHeight.value = 1230 * document.documentElement.clientWidth / 2560
                console.log(videoWidth.value)
            })
            //循环取帧
            loopFrame();
        })
        return {
            isdisplay,
            videoisplay,
            videoWidth,
            videoHeight
        }
    }
})
</script>

<style lang='stylus'>
.Tracker {
    width: 100% 
    height: 100%
    position: relative
    #face_panel {
        position: relative
        padding-left: 50px;
        width: 100% 
        height: 100%
        > span {
            color: black;
            position: absolute;
            top: 2%;
            left: 40%;
            font-size: 2vmin;
        }
        #video {
            position: absolute;
            top: 0;
            left:0
            border-radius: 6px;
            object-fit:fill;
        }
        #myCanvas {
            position: absolute;
            top: 0;
            left:0
            border: none;
            // width: 29.25rem
            // height: 23.0625rem
            width: 9.75rem
            height: 7.6875rem
            transform: scale(0.5);
        }
    }
}
.no-display {
    display: none    
}
.show-display {
    display: block    
}
.my-message {
    height: .4375rem
    .el-icon {
        width: .4375rem
        height: 100%
        svg {
            width: .3125rem
            height: .3125rem

        }
    }
    
}
</style>
```

