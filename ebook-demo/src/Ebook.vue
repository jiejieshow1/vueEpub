<template>
  <div class="ebook">
    <transition name="slide-down">
      <div class="title-wrapper" v-show="ifTitleAndMenuShow">
        <div class="left">
          <span class="icon-uniE900 icon"></span>
        </div>
        <div class="right">
          <div class="icon-wrapper">
            <span class="icon-2 icon"></span>
          </div>
          <div class="icon-wrapper">
            <span class="icon-4 icon"></span>
          </div>
          <div class="icon-wrapper">
            <span class="icon-5 icon"></span>
          </div>
        </div>
      </div>
    </transition>

    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTittleAndMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <div class="menu-wrapper" v-show="ifTitleAndMenuShow">
      <div class="icon-wrapper">
        <span class="icon-3 icon"></span>
      </div>
      <div class="icon-wrapper">
        <span class="icon-switch-ON-c icon"></span>
      </div>
      <div class="icon-wrapper">
        <span class="icon-1 icon"></span>
      </div>
      <div class="icon-wrapper">
        <span class="icon-a icon">A</span>
      </div>
    </div>
  </div>
</template>

<script>
/*引入epubs*/
import Epub from "epubjs";
/*下载地址*/
const DOWNLOAD_URL = "/static/zhaliezhi.epub";

//添加eslint忽略检查报错
/*eslint-disable space0before-function-paren */
export default {
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
  .title-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    width: 100%;
    height: px2rem(48);
    z-index: 101;
    background: white;
    box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, 15);
    .left {
      @include center;
      flex: 0 0 px2rem(60);
    }
    .right {
      flex: 1;
      display: flex;
      justify-content: flex-end;
      .icon-wrapper {
        flex: 0 0 px2rem(40);
        @include center;
      }
    }
    /* 加上"&"符号告诉css某个对象是同级的 */
    &.slide-down-enter {
      transform: translate3d(0, -100%, 0)
    }
    &.slide-down-enter-to {
      transform: translate3d(0, 0, 0)
    }
    &.slide-down-enter-active {
      transform: all .3s linear;
    }
  }
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
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    display: flex;
    width: 100%;
    height: px2rem(48);
    z-index: 101;
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 15);
    .icon-wrapper {
      flex: 1;
      @include center;
    }
    .icon-3 {
      font-size: px2rem(18);
    }
    .icon-a {
      font-size: px2rem(18);
    }
  }
}
</style>
