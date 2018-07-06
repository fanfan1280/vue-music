<template>
  <div class="singer">
    <list-view @select="selectSinger" :data="singers"></list-view>
    <router-view></router-view>
  </div>
</template>

<script>
  import {getSinger} from 'api/singer'
  import Singer from 'common/js/singer'
  import ListView from 'base/listView/listView'
  import {mapMutations} from 'vuex'

  const HOT_NAME = '热门歌手'
  const HOT_SINGER_LEN = 10
export default {
  name: 'singer',
  data() {
    return {
      singers: []
    }
  },
  created() {
    this._getSinger()
  },
  methods: {
    selectSinger(singer) {
      this.$router.push({
        path: `/singer/${singer.id}`
      })
      this.setSinger(singer)
      // this.$store.commit('SET_SINGER', singer)
    },
    _getSinger() {
      getSinger().then((res) => {
        if (res.code === 0) {
          this.singers = this._normalizeSinger(res.data.list)
          console.log(this.singers)
        }
      })
    },
    _normalizeSinger(list) {
      let map = {
        hot: {
          title: HOT_NAME,
          items: []
        }
      }
      list.forEach((item, index) => {
        if (index < HOT_SINGER_LEN) {
          map.hot.items.push(new Singer({
            name: item.Fsinger_name,
            id: item.Fsinger_mid
          }))
        }
        const key = item.Findex
        if (!map[key]) {
          map[key] = {
            title: key,
            items: []
          }
        }
        map[key].items.push(new Singer({
          name: item.Fsinger_name,
          id: item.Fsinger_mid
        }))
      })
      let hot = []
      let ret = []
      for (let key in map) {
        let val = map[key]
        if (val.title === HOT_NAME) {
          hot.push(map[key])
        } else if (val.title.match(/[a-zA-z]/)) {
          ret.push(map[key])
        }
      }
      ret.sort((a, b) => {
        return a.title.charCodeAt(0) - b.title.charCodeAt(0)
      })
      return hot.concat(ret)
    },
    ...mapMutations({
      setSinger: 'SET_SINGER'
    })
  },
  components: {
    ListView
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">

</style>
