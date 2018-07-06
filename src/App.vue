<template>
  <div id="App">
    <m-header></m-header>
    <Tab></Tab>
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
    <player></player>
  </div>
</template>

<script>
import MHeader from 'components/m-header/m-header'
import Tab from 'components/tab/tab'
import Player from 'components/player/player'
export default {
  components: {
    MHeader,
    Tab,
    Player
  },
  name: 'App'
}
</script>

<style scoped lang="stylus" rel="stylesheel/stylus">
  @import "common/stylus/index.styl";

</style>
