<template>
    <transition name="slider">
      <music-list :title="title" :bg-image="bgImage" :songs="songs"></music-list>
    </transition>
</template>

<script>
  import {mapGetters} from 'vuex'
  import {getSingerDetail} from 'api/singer'
  import {ERR_OK} from 'common/js/config'
  import {createSong} from 'common/js/song'
  import MusicList from 'components/music-list/music-list'

  export default {
    name: 'singer-detail',
    data() {
      return {
        songs: []
      }
    },
    computed: {
      title() {
        return this.singer.name
      },
      bgImage() {
        return this.singer.avatar
      },
      ...mapGetters([
        'singer'
      ])
    },
    created() {
      this._getDetail()
      console.log(this.singer)
    },
    methods: {
      _getDetail() {
        if (!this.singer.id) {
          this.$router.push('/singer')
          return
        }
        getSingerDetail(this.singer.id).then((res) => {
          if (res.code === ERR_OK) {
            this.songs = this._nomalizeSongs(res.data.list)
            console.log(this.songs)
          }
        })
      },
      _nomalizeSongs(list) {
        let ret = []
        list.forEach((item) => {
          let {musicData} = item
          ret.push(createSong(musicData))
        })
        return ret
      }
    },
    components: {
      MusicList
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable.styl"
  .slider-enter-active, .slider-leave-active
    transition  all 0.3s
  .slider-enter, .slider-leave-to
    -webkit-transform: translate3d(100%, 0, 0)
    -moz-transform: translate3d(100%, 0, 0)
    -ms-transform: translate3d(100%, 0, 0)
    -o-transform: translate3d(100%, 0, 0)
    transform: translate3d(100%, 0, 0)
</style>
