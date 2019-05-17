<template>
  <div class="page">
    <div class="page-content">
      <main class="main-content">
        <div class="demo">
          <slot name="demo"></slot>
        </div>
      </main>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'optional-demo',
    data() {
      return {
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .page
    position: fixed
    z-index: 20
    top: 0
    left: 0
    width: 100%
    height: 100%
    background: $color-white
    .page-content
      flex: 1 0 95%
      overflow: auto
      -webkit-overflow-scrolling: touch
      .main-content
        display: flex
        justify-content: space-between
        flex-wrap: wrap
        flex: 1 0 auto
        .demo
          width 100%
          .scroll-list-wrap
            height 100vh
            position relative
            border: 1px solid rgba(0, 0, 0, .1)
            border-radius: $radius-size-medium
            transform: rotate(0deg) // fix 子元素超出边框圆角部分不隐藏的问题
            overflow: hidden
</style>
