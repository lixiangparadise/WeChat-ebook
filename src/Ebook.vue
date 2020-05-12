<template>
 <div class="ebook">
     <!-- 标题栏 -->
     <transition name="slide-down">
     <div class="title-wrapper"  v-show="ifTitleAndMenuShow">
         <div class="left">
             <span class="icon-back icon"></span>
         </div>
         <div class="right">
             <div class="icon-wrapper">
                 <span class="icon-cart icon"></span>
             </div>
             <div class="icon-wrapper">
                 <span class="icon-person icon"></span>
             </div>
             <div class="icon-wrapper">
                 <span class="icon-more icon"></span>
             </div>
         </div>
     </div>
     </transition>
     <!-- 内容 -->
     <div class="read-wrapper">
         <!-- 电子书固定在此id上 -->
         <div id="read"></div>
         <!-- 左右进度浮层 -->
         <div class="mask">
             <div class="left" @click="prevPage"></div>
             <div class="center" @click="toggleTitleAndMenu"></div>
             <div class="right" @click="nextPage"></div>
         </div>
     </div>

     <!-- 底部栏 -->
     <transition name="slide-up">
     <div class="menu-wrapper" v-show="ifTitleAndMenuShow">
         <div class="icon-wrapper">
             <span class="icon-menu icon"></span>
         </div>
         <div class="icon-wrapper">
             <span class="icon-progress icon"></span>
         </div>
         <div class="icon-wrapper">
             <span class="icon-bright icon"></span>
         </div>
         <div class="icon-wrapper">
             <span class="icon-a icon">A</span>
         </div>
     </div>
     </transition>
 </div>
</template>

<script>
//导入epub
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
global.ePub = Epub
 export default {
  name: 'Ebook',
  data(){
      return{
          ifTitleAndMenuShow: false
      }
  },
  methods:{
      //点击中间内容区域则触发点击事件显示菜单栏
      toggleTitleAndMenu(){
          this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      },
      //翻页
      prevPage(){
          //调用rendition.prev上一页
          if(this.rendition){
              this.rendition.prev();
          }
      },
      nextPage(){
          //调用rendition.next下一页
          if(this.rendition){
              this.rendition.next();
          }
      },
      //电子书的解析和渲染
      showEpub(){
          //1.生成book对象
          this.book = new Epub(DOWNLOAD_URL);
        //   console.log(this.book);
          //2.生成rendition
          //通过book上的renderTo方法生成rendition对象，使用该对象上的dispaly函数进行对电子书的渲染
          //参数1.挂载的DOM ID 2.option
          this.rendition = this.book.renderTo('read',{
              //充满全屏
              width: window.innerWidth,
              height: window.innerHeight
          })
          console.log(this.rendition);
          //3.通过Rendtion.display()渲染电子书
          this.rendition.display();
      }
  },
  mounted(){
      this.showEpub()
  }
 }
</script>

<style lang='scss' scoped>
@import 'assets/styles/global';
.ebook{
    position: relative;
    .title-wrapper{
        position: absolute;
        top:0;
        left:0;
        width: 100%;
        z-index: 101;//层级越大在上面
        display: flex;
        height: px2rem(48);
        background-color: white;
        // 设置阴影
        box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .15);
        .left{
            flex: 0 0 px2rem(60);
            // 居中
            @include center;
        }
        .right{
            display: flex;
            flex:1;
            // 居右排列
            justify-content: flex-end;
            .icon-wrapper{
                flex: 0 0 px2rem(40);
                // 居中
                @include center;
                .icon-cart {
                    font-size: px2rem(22);
                }
            }
        }
        // // 添加& 表示和title-wrapper是同一级的
        // //等同于title-wrapper:slide-down-enter 作用在同一标签上
        // &.slide-down-enter, &.slide-down-leave-to{
        //     // 初始位置
        //     transform: translate3d(0, -100%, 0);
        // }
        // &.slide-down-enter-to, &.slide-down-leave{
        //     //最终位置
        //     transform: translate3d(0, 0, 0);
        // }
        // &.slide-down-enter-active, &.slide-down-leave-active{
        //     //过程
        //     transition: all, 0.3s linear;
        // }
    }
    .read-wrapper{
        .mask{
            position: absolute;
            top:0;
            left:0;
            z-index:100;
            display:flex;
            width: 100%;
            height: 100%;
            // background: yellow;
            .left{
                flex: 0 0 px2rem(100);
                // background-color: red;
            }
            .center{
                flex: 1;
                // background-color: orange;
            }
            .right{
                flex: 0 0 px2rem(100);
                // background-color: blue;
            }
        }
    }
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
}
 
</style>
