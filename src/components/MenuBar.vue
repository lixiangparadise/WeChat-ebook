<template>
  <div class="menu-bar">
     <!-- 底部栏 -->
     <transition name="slide-up">
        <div class="menu-wrapper" :class="{'hide-box-shadow': ifSettingShow || !ifTitleAndMenuShow}"
        v-show="ifTitleAndMenuShow">
            <div class="icon-wrapper">
                <span class="icon-menu icon"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-progress icon" @click="showSetting(2)"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-bright icon" @click="showSetting(1)"></span>
            </div>
            <div class="icon-wrapper">
                <span class="icon-a icon" @click="showSetting(0)">A</span>
            </div>
        </div>
     </transition>
     <transition name="slide-up">
        <div class="setting-wrapper" v-show="ifSettingShow">
            <!-- 字体选择 -->
            <div class="setting-font-size" v-show="showTag===0">
                <!-- 获取第一个字号 -->
                <div class="preview" :style="{fontSize: fontSizeList[0].fontSize + 'px'}">A</div>
               <div class="select">
                   <div class="select-wrapper"  v-for="(item, index) in fontSizeList" 
                    :key="index" @click="setFontSize(item.fontSize)">
                    <div class="line"></div>
                    <div class="point-warpper">
                        <!-- 显示小球 -->
                        <div class="point" v-show="defaultFontSize===item.fontSize">
                            <div class="small-point"></div>
                        </div>
                    </div>
                    <div class="line"></div>
                </div>
               </div>
                
                <!-- 获取最后一个字号 -->
                <div class="preview" :style="{fontSize: fontSizeList[fontSizeList.length-1].fontSize + 'px'}">A</div>
            </div>
            <!-- 设置主题 -->
            <div class="setting-theme" v-show="showTag===1">
                <div class="setting-theme-item" v-for="(item, index) in themeList"
                :key="index" @click="setTheme(index)">
                <!-- 如果是白色则加边框  否则不加边框 -->
                    <div class="preview" :style="{background: item.style.body.background}"
                    :class="{'no-border': item.style.body.background!=='#fff'}"></div>
                    <!-- 被选中时改变颜色 -->
                    <div class="text" :class="{'selected': index===defaultTheme}">{{item.name}}</div>
                </div>
            </div>

            <!-- 进度条 -->
            <div class="setting-progress" v-show="showTag===2">
                <div class="progress-wrapper">
                    <!-- 进度条 -->
                    <!-- $event.target.value 获取当前文本框的值 -->
                    <input type="range" class="progress"
                    max="100" min="0" step="1"
                    @change="onProgressChange($event.target.value)"
                    @input="onProgressInput($event.target.value)"
                    :value="progress"
                    :disabled = "!bookAvailable"
                    ref="progress"
                    />
                    
                </div>
                <div class="text-wrapper">
                    <span>{{bookAvailable ? progress+'%' : '加载中...'}}</span>
                </div>
            </div>
        </div>
     </transition>
  </div>
</template>

<script>
export default {
    props:{
        ifTitleAndMenuShow:{
            type:Boolean,
            default:false
        },
        fontSizeList: {
            type: Array
        },
        defaultFontSize: Number,
        themeList: Array,
        defaultTheme: Number,
        bookAvailable: Boolean
    },
    data(){
        return{
            ifSettingShow: false,
            showTag: 0,
            progress:0
        }
    },
    methods:{
        //进度条松开后出啊发事件，根据进度条数值跳转到指定位置
        onProgressChange(progress){
            this.$emit('onProgressChange', progress);
        },
        //拖动进度条时触发事件
        onProgressInput(progress){
            this.progress = progress;
            // 设置两个百分数分别对应.progress.background-size的两个参数
            this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
        },
        //设置主题
        setTheme(index){
            //调用父类的方法并且传递参数
            this.$emit('setTheme', index);
        },
        showSetting(tag){
            this.ifSettingShow = true
            this.showTag=tag
        },
        hideSetting(){
            this.ifSettingShow = false
        },
        //设置字号小球
        //使用父级的setFontSize方法并且传递参数
        setFontSize(fs){
            this.$emit('setFontSize', fs)
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global';
.menu-bar{
    .menu-wrapper{
        position: absolute;
        left:0;
        bottom: 0;
        z-index: 101;
        display: flex;
        width:100%;
        // justify-content: space-between;
        background-color: white;
        height: px2rem(48);
        // 设置向上阴影
        box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
        // 隐藏
        &.hide-box-shadow{
            box-shadow: none;
        }
        .icon-wrapper{
            flex:1;
            @include center;
            .icon-progress {
                font-size: px2rem(28);
            }
            .icon-bright {
                font-size: px2rem(24);
            }
        }
        // &.slide-up-enter, &.slide-up-leave-to{
        //     transform: translate3d(0, 100%, 0);
        // }
        // &.slide-up-enter-to, &.slide-up-leave{
        //     transform: translate3d(0, 0, 0);
        // }
        // &.slide-up-enter-active, &.slide-up-leave-active{
        //     transition: all, 0.3s linear;
        // }
    }
    .setting-wrapper{
        position: absolute;
        left:0;
        bottom: px2rem(48);
        width:100%;
        z-index:101;
        height:px2rem(60);
        background-color: white;
        box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
        .setting-font-size{
            display: flex;
            height:100%;
            .preview{
                flex: 0 0 px2rem(40);
                @include center //居中
            }
            .select{
                display:flex;
                flex:1;
                .select-wrapper{
                    flex:1;
                    // @include center;
                    align-items: center;
                    display: flex;
                    // 左右多余的线去掉
                    &:first-child{
                        .line{
                            &:first-child{
                                border-top:none;
                            }
                        }
                    }
                    &:last-child{
                        .line{
                            &:last-child{
                                border-top: none;
                            }
                        }
                    }
                    .line{
                        flex:1;
                        height:0;
                        border-top: px2rem(1) solid #ccc;
                    }
                    .point-warpper{
                        position: relative;
                        flex: 0 0 0;
                        width: 0;
                        height: px2rem(7);
                        border-left: px2rem(1) solid #ccc;
                        .point{
                            position: absolute;
                            top: px2rem(-8);
                            left: px2rem(-10);
                            width: px2rem(20);
                            height:px2rem(20);
                            border-radius:50%;
                            background-color: white;
                            border: px2rem(1) solid #ccc;
                            box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, .15);
                            @include center;
                        }
                        .small-point{
                            width: px2rem(5);
                            height: px2rem(5);
                            background: black;
                            border-radius: 50%;
                        }
                    }
                }
            }
            
        }
        .setting-theme{
            height:100%;
            // background-color: yellow;
            display: flex;
            .setting-theme-item{
                flex:1;
                display: flex;
                flex-direction: column;
                padding: px2rem(5);
                box-sizing: border-box;
                .preview{
                    flex:1;
                    border: px2rem(1) solid #ccc;
                    box-sizing: border-box;
                    &.no-border{
                        border:none;
                    }
                }
                .text{
                    flex: 0 0 px2rem(20);
                    font-size: px2rem(14);
                    color: #ccc;
                    @include center;
                    &.selected {
                        color:#333;
                    }
                }
            }
        }
        .setting-progress{
            position: relative;
            height:100%;
            width: 100%;
            .progress-wrapper{
               height:100%; 
                width: 100%;
                @include center;
                padding: 0 px2rem(30);
                box-sizing: border-box;
                .progress{
                    width: 100%;
                    //改变浏览器默认风格
                    -webkit-appearance: none;
                    height: px2rem(2);
                    background: -webkit-linear-gradient(#999, #999) no-repeat,  #ddd;
                    background-size: 0 100%;
                    &:focus {
                        outline: none;
                    }
                    // 设置range滑块的具体样式 中间的圆形
                    &::-webkit-slider-thumb {
                        -webkit-appearance: none;
                        height: px2rem(20);
                        width: px2rem(20);
                        border-radius: 50%;
                        background: white;
                        box-shadow: 0 4px 4px 0 rgba(0, 0, 0, .15);
                        border: px2rem(1) solid #ddd;
                    }
                }
            }
            .text-wrapper{
                position: absolute;
                left:0;
                bottom: 0;
                width: 100%;
                color: #333;
                font-size:px2rem(12);
                text-align: center;
            }
        }



    }
    
}
    

</style>