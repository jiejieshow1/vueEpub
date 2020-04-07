<template>
    <transition name="slde-right">
      <div class="leftWrapper" v-show="showContentWrapper"></div>
    </transition>

</template>
<script>
export default {
    props(){

    },

  data() {
    return {
        showContentWrapper : false
    };
  },
  methods: {
      showContent(){
          this.showContentWrapper = !showContentWrapper
      }
  },
};

</script>

<style lang='scss' scoped>
@import "../assets/styles/global";
  .leftWrapper {
    position: absolute;
    height: 100%;
    left: 0;
    width: 60%;
    background: red;
  }

</style>