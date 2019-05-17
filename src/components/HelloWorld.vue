<template>
  <div>
    <div class="scroll-list-wrap" slot="demo">
      <scroll ref="scroll"
        :data="items"
        :scrollbar="scrollbarObj"
        :pullDownRefresh="pullDownRefreshObj"
        :pullUpLoad="pullUpLoadObj"
        :startY="parseInt(startY)"
        @pullingDown="onPullingDown"
        @pullingUp="onPullingUp"
        @click="clickItem">
      </scroll>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  import OptionalDemo from './optional-demo/optional-demo.vue'
  import Scroll from './scroll/scroll.vue'
  import SwitchOption from './switch-option/switch-option.vue'
  import InputOption from './input-option/input-option.vue'
  import SelectOption from './select-option/select-option.vue'

  import { ease } from '../common/ease'

  export default {
    data() {
      return {
        scrollbar: true,
        scrollbarFade: true,
        pullDownRefresh: true,
        pullDownRefreshThreshold: 90,
        pullDownRefreshStop: 40,
        pullUpLoad: true,
        pullUpLoadThreshold: 0,
        pullUpLoadMoreTxt: '加载更多',
        pullUpLoadNoMoreTxt: '没有更多数据了',
        startY: 0,
        scrollToX: 0,
        scrollToY: -200,
        scrollToTime: 700,
        scrollToEasing: 'bounce',
        scrollToEasingOptions: ['bounce', 'swipe', 'swipeBounce'],
        items: [],
        itemIndex: 0
      }
    },
    created() {
      for (let i = 0; i < 2; i++) {
        this.items.push('我是第 ' + ++this.itemIndex + ' 行')
      }
    },
    components: {
      OptionalDemo,
      Scroll,
      SwitchOption,
      InputOption,
      SelectOption
    },
    watch: {
      scrollbarObj: {
        handler() {
          this.rebuildScroll()
        },
        deep: true
      },
      pullDownRefreshObj: {
        handler(val) {
          const scroll = this.$refs.scroll.scroll
          if (val) {
            scroll.openPullDown()
          } else {
            scroll.closePullDown()
          }
        },
        deep: true
      },
      pullUpLoadObj: {
        handler(val) {
          const scroll = this.$refs.scroll.scroll
          if (val) {
            scroll.openPullUp()
          } else {
            scroll.closePullUp()
          }
        },
        deep: true
      },
      startY() {
        this.rebuildScroll()
      }
    },
    computed: {
      scrollbarObj: function () {
        return this.scrollbar ? {fade: this.scrollbarFade} : false
      },
      pullDownRefreshObj: function () {
        return this.pullDownRefresh ? {
          threshold: parseInt(this.pullDownRefreshThreshold),
          stop: parseInt(this.pullDownRefreshStop)
        } : false
      },
      pullUpLoadObj: function () {
        return this.pullUpLoad ? {
          threshold: parseInt(this.pullUpLoadThreshold),
          txt: {more: this.pullUpLoadMoreTxt, noMore: this.pullUpLoadNoMoreTxt}
        } : false
      }
    },
    methods: {
      scrollTo() {
        // expect a number, not string
        const scrollToTime = Number(this.scrollToTime)
        const scrollToY = Number(this.scrollToY)
        const scrollToX = Number(this.scrollToX)
        this.$refs.scroll.scrollTo(scrollToX, scrollToY, scrollToTime, ease[this.scrollToEasing])
      },
      autoPullDownRefresh () {
        this.$refs.scroll.autoPullDownRefresh()
      },
      onPullingDown() {
        // 模拟更新数据
        console.log('pulling down and load data')
        setTimeout(() => {
          if (this._isDestroyed) {
            return
          }
          if (Math.random() > 0.5) {
            // 如果有新数据
            this.items.unshift('我是新数据: ' + +new Date())
          } else {
            // 如果没有新数据
            this.$refs.scroll.forceUpdate()
          }
        }, 2000)
      },
      onPullingUp() {
        // 更新数据
        console.log('pulling up and load data')
        setTimeout(() => {
          if (this._isDestroyed) {
            return
          }
          if (Math.random() > 0.5) {
            // 如果有新数据
            let newPage = []
            for (let i = 0; i < 10; i++) {
              newPage.push('我是第 ' + ++this.itemIndex + ' 行')
            }

            this.items = this.items.concat(newPage)
          } else {
            // 如果没有新数据
            this.$refs.scroll.forceUpdate()
          }
        }, 1500)
      },
      clickItem() {
        this.$router.back()
      },
      updateScrollbar(val) {
        this.scrollbar = val
      },
      updateScrollbarFade(val) {
        this.scrollbarFade = val
      },
      updatePullDownRefresh(val) {
        this.pullDownRefresh = val
      },
      updatePullDownRefreshThreshold(val) {
        this.pullDownRefreshThreshold = val
      },
      updatePullDownRefreshStop(val) {
        this.pullDownRefreshStop = val
      },
      updatePullUpLoad(val) {
        this.pullUpLoad = val
      },
      updatePullUpLoadThreshold(val) {
        this.pullUpLoadThreshold = val
      },
      updatePullUpLoadMoreTxt(val) {
        this.pullUpLoadMoreTxt = val
      },
      updatePullUpLoadNoMoreTxt(val) {
        this.pullUpLoadNoMoreTxt = val
      },
      updateStartY(val) {
        this.startY = val
      },
      updateScrollToX(val) {
        this.scrollToX = val
      },
      updateScrollToY(val) {
        this.scrollToY = val
      },
      updateScrollToTime(val) {
        this.scrollToTime = val
      },
      updateScrollToEasing(val) {
        this.scrollToEasing = val
      },
      rebuildScroll() {
        this.nextTick(() => {
          this.$refs.scroll.destroy()
          this.$refs.scroll.initScroll()
        })
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
.scroll-list-wrap {
  position fixed
  width 100%
  height 100%
  left 0
  top 0
}
</style>
