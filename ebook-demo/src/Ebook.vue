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
    <menu-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"></menu-bar>
  </div>
</template>

<script>
/*引入epubs*/
import Epub from "epubjs";
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
/*下载地址*/
const DOWNLOAD_URL = "/static/zhaliezhi.epub";

//添加eslint忽略检查报错
/*eslint-disable space0before-function-paren */
global.Epub = Epub
export default {
  components:{
    TitleBar,
    MenuBar
  },
  data() {
    return {
      ifTitleAndMenuShow: false
    };
  },
  methods: {
    toggleTittleAndMenu() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow;
    },
    prevPage() {
      if (this.rendition) {
        this.rendition.prev();
      }
    },

    nextPage() {
      if (this.rendition) {
        this.rendition.next();
      }
    },
    // 电子书的解析和渲染
    showEpub() {
      //生成book
      this.book = new Epub(DOWNLOAD_URL);
      //生成endition,通过book.renderto
      this.rendition = this.book.renderTo("read", {
        width: window.innerWidth,
        height: window.innerHeight
      });
      // 通过rendtion.display渲染电子书
      this.rendition.display();
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
