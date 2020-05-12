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
                <span class="icon-progress icon"></span>
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
        defaultTheme: Number
    },
    data(){
        return{
            ifSettingShow: false,
            showTag: 0
        }
    },
    methods:{
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




    }
    
}
    

</style>