<template>
  <div id="app">
    <m-header></m-header>
    <tab></tab>
    <!-- 缓存组件 -->
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
    <!-- <player></player> -->
  </div>
</template>
<script type='text/ecmajavascript'>
  import MHeader from 'components/m-header/m-header'
  // import Player from 'components/player/player'
  import Tab from 'components/tab/tab'
  import Play from 'components/player/player'
  
  export default{
    components: {
      MHeader,
      Tab
    }
  }
</script>

<style scoped lang="sass">
</style>