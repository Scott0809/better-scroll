<template>
  <div class="slide-vertical">
    <div class="vertical-wrapper">
      <div class="slide-vertical-wrapper" ref="slide">
        <div class="slide-vertical-content">
          <div v-for="num in nums" :key="num" class="slide-page" :class="'page' + num">page {{num}}</div>
        </div>
      </div>
      <div class="dots-wrapper">
        <span
          class="dot"
          v-for="num in nums"
          :key="num"
          :class="{'active': currentPageIndex === (num - 1)}"></span>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from '@better-scroll/core'
  import Slide from '@better-scroll/slide'

  BScroll.use(Slide)

  export default {
    data() {
      return {
        nums: 4,
        currentPageIndex: 0
      }
    },
    mounted() {
      this.init()
    },
    beforeDestroy() {
      this.slide.destroy()
    },
    methods: {
      init() {
        this.slide = new BScroll(this.$refs.slide, {
          scrollX: false,
          scrollY: true,
          slide: {
            threshold: 100
          },
          useTransition: true,
          momentum: false,
          bounce: false,
          stopPropagation: true
        })
        this.slide.on('scrollEnd', this._onScrollEnd)
      },
      _onScrollEnd() {
        let pageIndex = this.slide.getCurrentPage().pageY
        this.currentPageIndex = pageIndex
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">

.slide-vertical
  height 100%
  &.view
    padding 0
    height 100%
  .vertical-wrapper
    position relative
    height 100%
    font-size 0
  .slide-vertical-wrapper
    height 100%
    overflow hidden
    .slide-page
      display inline-block
      width 100%
      line-height 200px
      text-align center
      font-size 26px
      transform translate3d(0,0,0)
      backface-visibility hidden
      &.page1
        background-color #D6EADF
      &.page2
        background-color #DDA789
      &.page3
        background-color #C3D899
      &.page4
        background-color #F2D4A7
  .dots-wrapper
    position absolute
    right 4px
    top 50%
    transform translateY(-50%)
    .dot
      display block
      margin 4px 0
      width 8px
      height 8px
      border-radius 50%
      background #eee
      &.active
        height  20px
        border-radius 5px

</style>
