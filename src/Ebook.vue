<template>
 <div class="ebook">
     <div class="read-wrapper">
         <!-- 电子书固定在此id上 -->
         <div id="read"></div>
         <!-- 左右进度浮层 -->
         <div class="mask">
             <div class="left" @click="prevPage"></div>
             <div class="center"></div>
             <div class="right" @click="nextPage"></div>
         </div>
     </div>
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

      }
  },
  methods:{
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
}
 
</style>
