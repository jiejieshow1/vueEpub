<template>
  <transition name="slide-up">
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
@import "../src/assets/styles/global";
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
</style>