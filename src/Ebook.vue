<template>
 <div class="ebook">
     <!-- 标题栏 -->
     <title-bar :ifTitleAndMenuShow = "ifTitleAndMenuShow"></title-bar>
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
     <!-- 底部 -->
    <menu-bar ref="menuBar" :ifTitleAndMenuShow = "ifTitleAndMenuShow"
                :fontSizeList = "fontSizeList"
                :defaultFontSize = "defaultFontSize"
                @setFontSize = "setFontSize"
                :themeList = "themeList"
                :defaultTheme = "defaultTheme"
                @setTheme = "setTheme"
    ></menu-bar>
     
 </div>
</template>

<script>
//导入组件
import MenuBar from '@/components/MenuBar'
import TitleBar from '@/components/TitleBar'
//导入epub
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
global.ePub = Epub
 export default {
  name: 'Ebook',
  data(){
      return{
          ifTitleAndMenuShow: false,
          fontSizeList:[
              {fontSize: 12},
              {fontSize: 14},
              {fontSize: 16},
              {fontSize: 18},
              {fontSize: 20},
              {fontSize: 22},
              {fontSize: 24}
          ],
          defaultFontSize: 16,
          //主题
          themeList: [
            {
            name: 'default',
            style: {
                body: {
                'color': '#000', 'background': '#fff'
                }
            }
            },
            {
            name: 'eye',
            style: {
                body: {
                'color': '#000', 'background': '#ceeaba'
                }
            }
            },
            {
            name: 'night',
            style: {
                body: {
                'color': '#fff', 'background': '#000'
                }
            }
            },
            {
            name: 'gold',
            style: {
                body: {
                'color': '#000', 'background': 'rgb(241, 236, 226)'
                }
            }
            }
        ],
        defaultTheme: 0
      }
  },
  components:{
      MenuBar,
      TitleBar
  },
  methods:{
      setTheme(index){
          this.themes.select(this.themeList[index].name);
          this.defaultTheme = index; //保存主题
      },
      //注册主题
      registerTheme(){
          this.themeList.forEach(theme=>{
              this.themes.register(theme.name, theme.style)
          })
      },
      setFontSize(fs){
          this.defaultFontSize = fs;
          if(this.themes){
              this.themes.fontSize(fs+'px')
          }
      },
      //点击中间内容区域则触发点击事件显示菜单栏
      toggleTitleAndMenu(){
          this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow,
        //   调用子级函数
          this.$refs.menuBar.hideSetting();
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
        //   console.log(this.rendition);
          //3.通过Rendtion.display()渲染电子书
          this.rendition.display();
          //获取Theme对象
          this.themes = this.rendition.themes
          //设置默认字体
          this.setFontSize(this.defaultFontSize)
          //this.themes.register(theme name, theme style) 将主题注册到对象中
          //this.select(theme name) 通过主题名字修改
          //注册主题
          this.registerTheme();
          //   this.themes.select('night');
          // 设置默认主题
          this.setTheme(this.defaultTheme);
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
