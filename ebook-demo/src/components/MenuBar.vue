<template>
<!-- div 后方加上icon 为在global定义的公共样式 -->
  <div class="menu-bar">
    <transition name="slide-up">
      <div
        class="menu-wrapper"
        :class="{'hide-box-shadow':ifSettingShow || !ifTitleAndMenuShow}"
        v-show="ifTitleAndMenuShow"
      >
        <div class="icon-wrapper">
          <span class="icon-3 icon" @click="hideContent"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-switch-ON-c icon" @click="showSetting(2)"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-1 icon" @click="showSetting(1)"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-a icon" @click="showSetting(0)">A</span>
        </div>
      </div>
    </transition>
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifSettingShow">
        <div class="setting-font-size" v-if="showTag === 0">
          <div class="preview" :style="{fontSize:fontSizeList[0].fontSize + 'px'}">A</div>
          <div class="select">
            <div
              class="select-wrapper"
              v-for="(item,index) in fontSizeList"
              :key="index"
              @click="setFontSize(item.fontSize)"
            >
              <div class="line"></div>
              <div class="point-wrapper">
                <div class="point" v-show="defaultFontSize === item.fontSize">
                  <div class="small-point"></div>
                </div>
              </div>
              <div class="line"></div>
            </div>
          </div>
          <div
            class="preview"
            :style="{fontSize:fontSizeList[fontSizeList.length - 1].fontSize + 'px'}"
          >A</div>
        </div>
        <div class="setting-theme" v-else-if="showTag === 1">
          <div
            class="setting-theme-item"
            v-for="(item, index) in themeList"
            :key="index"
            @click="setTheme(index)"
          >
            <div
              class="preview"
              :style="{background: item.style.body.background}"
              :class="{'no-border': item.style.body.background != '#fff'}"
            ></div>
            <div class="text" :class="{'selected': index === defaultTheme}">{{item.name}}</div>
          </div>
        </div>
        <div class="setting-progress" v-else-if="showTag === 2">
          <div class="progress-wrapper">
            <input
              class="progress"
              type="range"
              max="100"
              min="0"
              setp="1"
              @change="onProgressChange($event.target.value)"
              @input="onProgressInput($event.target.value)"
              :value="progress"
              :disabled="!bookAvailable"
              ref="progress"
            />
          </div>
          <div class="text-wrapper">
            <span>{{bookAvailable ? progress + '%' : '加载中...'}}</span>
          </div>
        </div>
      </div>
    </transition>
    <transition name = "fade">
      <div class="content-mask" v-show="ifShowContent" @click="hideContent"></div>
        <Content></Content>>
    </transition>
  </div>
</template>

<script>
import Content from "@/components/Content";

export default {
  props: {
    fontSizeList: Array, 
    defaultFontSize: Number,
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    themeList: Array,
    defaultTheme: Number,
    bookAvailable: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      progress: 0,
      showTag: 0,
      ifSettingShow: false,
      ifShowContent: false,
      navigation: Object
    };
  }, 
  methods: {
    hideContent(){
      this.ifShowContent = !this.ifShowContent
    },
    // showContent(){

    // },
    jumpTo(target){
      this.$emit('jumpTo',target)
    },
    //拖动进度条时触发时间
    onProgressInput(progress) {
      this.progress = progress;
      // question ``符号是啥?  转义符:将任何变量强转为str
      this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`;
    },
    //进度条松开后触发时间,根据进度条跳转到指定位置
    onProgressChange(progress) {
      this.$emit("onProgressChange", progress);
    },
    setTheme(index) {
      this.$emit("setTheme", index);
    },
    setFontSize(fontSize) {
      // $emit 子组件调用父组件方法
      this.$emit("setFontSize", fontSize);
    },
    showSetting(tag) {
      this.ifSettingShow = !this.ifSettingShow;
      this.showTag = tag;
    },
    hideSetting() {
      this.ifSettingShow = false;
    }
  }
};
</script>

<style lang='scss' scoped>
@import "../assets/styles/global";

.menu-bar {
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    display: flex;
    width: 100%;
    height: px2rem(48);
    z-index: 101;
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    &.hide-box-shadow {
      box-shadow: none;
    }
    .icon-wrapper {
      //question : flex 1 ?          flex: 0, 0, 0, px2rem(100) 
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
  .setting-wrapper {
    z-index: 101;
    position: absolute;
    bottom: px2rem(48);
    width: 100%;
    height: px2rem(60);
    left: 0;
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    .setting-font-size {
      height: 100%;
      display: flex;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
      }
      .select {
        display: flex;
        flex: 1;
        .select-wrapper {
          flex: 1;
          display: flex;
          align-items: center;
          &:first-child {
            .line {
              &:first-child {
                border-top: none;
              }
            }
          }
          &:last-child {
            .line {
              &:last-child {
                border-top: none;
              }
            }
          }
          .line {
            flex: 1;
            height: 0;
            border-top: px2rem(1) solid #ccc;
          }
          .point-wrapper {
            position: relative;
            flex: 0 0 0;
            width: 0;
            height: px2rem(7);
            border-left: px2rem(1) solid #ccc;
            .point {
              position: absolute;
              top: px2rem(-8);
              left: px2rem(-10);
              width: px2rem(20);
              height: px2rem(20);
              border-radius: 50%;
              background: white;
              border: px2rem(1) solid #ccc;
              box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, 0.15);
              @include center;
              .small-point {
                width: px2rem(5);
                height: px2rem(5);
                background: black;
                border-radius: 50%;
              }
            }
          }
        }
      }
    }
    .setting-theme {
      background: yellow;
      height: 100%;
      display: flex;
      .setting-theme-item {
        flex: 1;
        display: flex;
        //横向布局
        flex-direction: column;
        padding: px2rem(5);
        box-sizing: border-box;
        .preview {
          flex: 1;
          border: px2rem(1) solid #ccc;
          box-sizing: border-box;
          // 通过绑定class来修改个别参数
          &.no-border {
            border: none;
          }
        }
        .text {
          flex: 0 0 px2rem(20);
          font-size: px2rem(14);
          color: #ccc;
          @include center;
          &.selected {
            color: #333;
          }
        }
      }
    }
    .setting-progress {
      position: relative;
      width: 100%;
      height: 100%;
      .progress-wrapper {
        width: 100%;
        height: 80%;
        @include center;
        padding: 0 px2rem(30);
        box-sizing: border-box;
        .progress {
          width: 100%;
          //覆盖默认样式
          -webkit-appearance: none;
          height: px2rem(2);
          //通过以下方式覆盖滚动条左右颜色
          background: -webkit-linear-gradient(#999, #999) no-repeat, #ddd;
          background-size: 0 100%;
          &:focus {
            outline: none;
          }
          // question ::-webkit-slider-thumb??  是一个样式控件
          &::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: px2rem(20);
            width: px2rem(20);
            border-radius: 50%;
            background: white;
            box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.15);
            border: px2rem(1) solid #ddd;
          }
        }
      }
      .text-wrapper {
        flex: 0 0 px2rem(20);
        font-size: px2rem(14);
        @include center;
      }
    }
  }

  .content-mask{
    position: absolute;
top: 0;
left: 0;
z-index: 101;
display: flex;
width: 100%;
height: 100%;
background: rgba(51, 51, 51, 0.8);
  }
}
</style>