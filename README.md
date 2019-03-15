# wedpage
测试网站
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="UTF-8">
    <script src="./js/vue.min.js"></script>
    <script src="./js/jquery-3.3.1.min.js"></script>
    <script src="./js/videoApi.json"></script>
    <link rel="stylesheet" href="./css/common.css">
    <title>vip视频解析</title>
</head>

<body>
    <div id="app">
        <div class="api-select">
            <input type="text" v-model.String="videoSrc" v-focus>
            <div class="apiList">
                <span @click.stop="toggleList">接口{{ getNowApiName }}</span>

                <ul v-if="isShow">
                    <li v-for="(item,index) in api" @click="selectApi(index)">接口 {{ index+1 }}</li>
                </ul>
            </div>
            <p class="tips">{{api.length}}个接口任君选择，解析不了请切换接口</p>
        </div>
        <iframe id="videos" src="" frameborder="0" :width="width" :height="height"></iframe>
    </div>
    <script>

    new Vue({
        el: '#app',
        data: {
            api:api,
            nowIndex:0,
            videoSrc:"https://v.qq.com/x/cover/p8bvvfh82dqrqgq.html",
            isShow:false,
            width:"",
            height:"",
        },
        methods: {
            toggleList:function(){
                if(this.isShow == false){
                    this.isShow = true;
                } else {
                    this.isShow = false;
                }
            },
            selectApi:function(index){
                this.nowIndex = index;
                this.toggleList();
            },
        },
        computed: {
            getNowApiName:function(){
                return this.nowIndex +1 ;
            },
            getApi:function(){
                return this.api[this.nowIndex] + this.videoSrc ;
            }
        },
        watch:{
            videoSrc:function(){
                document.getElementById("videos").src = this.getApi;
            },
            nowIndex:function(){
                document.getElementById("videos").src = this.getApi;
            }
        },
        directives:{
            focus:{
                inserted:function(el){
                    el.focus();
                }
            }
        },
        mounted:function(){
            let that = this;
            // 点击空白区域，列表消失
            document.documentElement.addEventListener("click",function(){
                that.isShow = false;
            })
            // 计算屏幕长宽
            this.width = $(window).width()+ "px";
            this.height = ($(window).height() - 50) + "px";
            // 打开默认播放默认视频
            document.getElementById("videos").src = this.getApi;
        }
    })

    </script>
</body>

</html>
