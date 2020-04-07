<template>
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
</template>
<script>
export default {
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    }
  }
};
</script>

<style lang='scss' scoped>
    @import '../assets/styles/global';

    .title-wrapper {
        position: absolute;
        top: 0;
        left: 0;
        display: flex;
        width: 100%;
        height: px2rem(48);
        z-index: 101;
        background: white;
        box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .15);

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
    }
</style>