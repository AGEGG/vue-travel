<template>
  <div>
    <div class="search">
      <input type="text" v-model="keyword" class="search-input" placeholder="输入城市名或拼音">
    </div>
    <div class="search-content" ref="search" v-show="keyword">
      <!--双向绑定keyword-->
      <ul>
        <!--遍历找到的城市-->
        <li class="search-item border-bottom" v-for="(city,index) in cityList" :key="index" @click="HandleCity(city)">{{city}}</li>
        <!--没有找到时的显示-->
        <li class="search-item border-bottom" v-show="hasCity">
          没有找到匹配数据
        </li>
      </ul>
    </div>
  </div>

</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: 'CitySearch',
  props: {
    cities: Object
  },
  data () {
    return {
      keyword: '',
      cityList: [],
      // 函数节流
      timer: null
    }
  },
  methods: {
    HandleCity (city) {
      this.$store.commit('changeCity', city)
      console.log(city)
      this.$router.push('/')
    }
  },
  computed: {
    hasCity () {
      return !this.cityList.length
    }
  },
  watch: {
    keyword () {
      if (this.timer) {
        clearTimeout(this.timer)
      }
      this.timer = setTimeout(() => {
        if (!this.keyword) {
          this.cityList = []
          return
        }
        const result = []
        for (let i in this.cities) {
          this.cities[i].forEach((value) => {
            if (value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1) {
              result.push(value.name)
            }
          })
          this.cityList = result
        }
      }, 100)
    }
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.search)
  }
}

</script>

<style lang="stylus" scoped>
@import "~styles/varibles.styl"
  .search
    height .72rem
    padding 0 .1rem
    background $bgColor
    .search-input
      padding 0 .1rem
      box-sizing border-box
      height .62rem
      line-height .62rem
      width 100%
      text-align center
      border-radius .06rem
      color #666
  .search-content
    overflow hidden
    background #eee
    position absolute
    top 1.58rem
    left 0
    right 0
    z-index: 1
    bottom 0
    .search-item
      line-height .62rem
      padding-left .2rem
      color #666
      background #fff
</style>
