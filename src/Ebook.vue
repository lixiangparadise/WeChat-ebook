<template>
 <div class="ebook">
     <div class="read-wrapper">
         <!-- 电子书固定在此id上 -->
         <div id="read"></div>
 
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

 
</style>
