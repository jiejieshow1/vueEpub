<template>
  <div class="ebook">
    <!-- 将iftitleandmenushow这个变量传入 -->
    <title-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"></title-bar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTittleAndMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <!-- ref: 此处定义的意义是定义menuBar给当前类调用menu-bar的方法使用 -->
    <menu-bar
      :ifTitleAndMenuShow="ifTitleAndMenuShow"
      :fontSizeList="fontSizeList"
      :defaultFontSize="defaultFontSize"
      @setFontSize="setFontSize"
      :themeList="themeList"
      :defaultTheme="defaultTheme"
      @setTheme="setTheme"
      :bookAvailable="bookAvailable"
      @onProgressChange="onProgressChange"
      @jumpTo="jumpTo"
      :navigation="navigation"
      ref="menuBar"
    ></menu-bar>
  </div>
</template>

<script>
/*引入epubs*/
import Epub from "epubjs";
import TitleBar from "@/components/TitleBar";
import MenuBar from "@/components/MenuBar";
/*下载地址*/
const DOWNLOAD_URL = "/static/zhaliezhi.epub";

//添加eslint忽略检查报错
/*eslint-disable space0before-function-paren */
global.Epub = Epub;
export default {
  components: {
    TitleBar,
    MenuBar
  },
  data() {
    return {
      ifTitleAndMenuShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: "defalut",
          style: {
            body: {
              color: "#000",
              background: "#fff"
            }
          }
        },
        {
          name: "eye",
          style: {
            body: {
              color: "#000",
              background: "#ceeaba"
            }
          }
        },
        {
          name: "night",
          style: {
            body: {
              color: "#fff",
              background: "#000"
            }
          }
        },
        {
          name: "glod",
          style: {
            body: {
              color: "#000",
              background: "rgb(241, 236, 226)"
            }
          }
        }
      ],
      defaultTheme: 0,
      //图书是否处于可用状态
      bookAvailable: false
    };
  },
  methods: {
    jumpTo(href) {
      this.book.display(href);
      this.hidTitleAndMenu();
    },
    hideTitleAndMenu() {
      //隐藏标题栏和菜单栏
      this.ifTitleAndMenuShow = false;
      //隐藏菜单栏弹出的设置栏
      this.$refs.menuBar.hideSetting();
      this.$refs.menuBar, hideContent();
    },
    onProgressChange(progress) {
      const percentage = progress / 100;
      const location =
        percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0;
      this.rendition.display(location);
    },
    setTheme(index) {
      this.themes.select(this.themeList[index].name);
      this.defaultTheme = index;
    },
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style);
      });
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize;
      if (this.themes) {
        this.themes.fontSize(fontSize + "px");
      } else {
      }
    },
    toggleTittleAndMenu() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow;
      if (!this.ifTitleAndMenuShow) {
        // this.$refs.menuBar.menuBar.background = 'black';
        this.$refs.menuBar.hideSetting();
      }
    },
    prevPage() {
      if (this.rendition) {
        this.rendition.prev();
      }
    },

    nextPage() {
      console.log(this.book);
      if (this.rendition) {
        this.rendition.next();
      }
    },
    // 电子书的解析和渲染
    showEpub() {
      //生成book
      this.book = new Epub(DOWNLOAD_URL);
      // question : rendition? display
      //生成rendition,通过book.renderto
      var test = this.book;
      // debugger
      //电子书根据挂载到read 的dom下面
      this.rendition = this.book.renderTo("read", {
        width: window.innerWidth,
        height: window.innerHeight
      });
      // 通过rendtion.display渲染电子书
      this.rendition.display();
      //获取theme对象
      // question : themes是什么,没有自动补全,怎么知道获取这个对象是否正确?
      this.themes = this.rendition.themes;
      //设置默认字体
      this.setFontSize(this.defaultFontSize);
      //this.thmems.register(name, styles)通过register注册名称与类型
      //this.themes.select(name) 最后通过select配置主题
      this.registerTheme();
      this.setTheme(this.defaultTheme);
      // this.themes.select('night')
      //获取loncation对象
      // 通过epubjs的 ready钩子函数,then为异步方法
      this.book.ready
        .then(() => {
          this.navigation = this.book.navigation;
          return this.book.locations.generate();
        })
        .then(result => {
          // console.log(result)
          this.locations = this.book.locations;
          this.bookAvailable = true;
          // this.onProgressChange(50)
        });
    }
  },
  mounted() {
    this.showEpub();
  }
};
</script>
<style lang= 'scss' scoped>
@import "assets/styles/global";

.ebook {
  position: relative;

  .read-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 100;
      width: 100%;
      height: 100%;
      display: flex;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1;
      }
      .right {
        flex: 0 0 px2rem(100);
      }
    }
  }
 
}
</style>
