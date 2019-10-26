<template>
  <div class="header">
    城市选择
    <router-link to="/">
      <div class="iconfont header-back">&#xe624;</div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'CityHeader'
}

</script>

<style lang="stylus" scoped>
  @import "~styles/varibles.styl"
  .header
    position relative
    overflow hidden
    height $headerHeight
    line-height $headerHeight
    color #ffffff
    background $bgColor
    text-align center
    font-size .32rem
    .header-back
      color #ffffff
      top 0
      left: 0
      font-size .4rem
      width .64rem
      position absolute
      text-align center
</style>
