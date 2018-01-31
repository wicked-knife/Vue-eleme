<template>
  <div class="line_title">
    <div class="line"></div><div class="title">{{title}}</div><div class="line"></div>
  </div>
</template>

<script>
export default {
  props: {
    title: {}
  }
}
</script>

<style lang="scss" scoped>
  .line_title{
    width: 100%;
    display: flex;
    box-sizing: border-box;
    position: relative;
    .line{
      border-bottom: 1px solid rgba(255,255,255,.2);
      position: relative;
      top:-6px;
      flex:1;
    }
    .title{
      margin:0 12px;
      font-size: 14px;line-height: 14px;
    }
  }
</style>
