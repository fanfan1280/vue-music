<template>
    <transition name="slider">
      <div class="singer-detail"></div>
    </transition>
</template>

<script>
  import {mapGetters} from 'vuex'

  export default {
    name: 'singer-detail',
    computed: {
      ...mapGetters([
        'singer'
      ])
    },
    created() {
      // console.log(this.singer)
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable.styl"
  .singer-detail
    position fixed
    z-index 100
    top:0
    left:0
    right:0
    bottom 0
    background $color-background
  .slider-enter-active, .slider-leave-active
    transition  all 0.3s
  .slider-enter, .slider-leave-to
    -webkit-transform: translate3d(100%, 0, 0)
    -moz-transform: translate3d(100%, 0, 0)
    -ms-transform: translate3d(100%, 0, 0)
    -o-transform: translate3d(100%, 0, 0)
    transform: translate3d(100%, 0, 0)
</style>
