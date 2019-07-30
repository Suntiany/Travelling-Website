<template>
  <div>
     <home-header></home-header>
     <home-swiper :list="swiperList"></home-swiper> 
     <home-icons :list="iconList"></home-icons>
     <home-recommend :list="recommendList"></home-recommend>
     <home-weekend :list="weekendList"></home-weekend>
  </div>
</template>

<script>
import HomeHeader from './Components/Header'
import HomeSwiper from './Components/Swiper'
import HomeIcons from './Components/icons'
import HomeRecommend from './Components/Recommend'
import HomeWeekend from './Components/Weekend'
import axios from 'axios'
import { mapState } from 'vuex'
export default {
  name: 'Home',
  components:{
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  data () {
    return {
      swiperList: [],
      iconList: [],
      recommendList: [],
      weekendList: [],
      lastCity: ''
    }
  },
  computed: {
    ...mapState(['city'])
  },
  methods: {
     getHomeInfo () {
       axios.get('/api/index.json?city=' + this.city)
        .then(this.getHomeInfoSucc)
     },
     getHomeInfoSucc (res) {
       res = res.data 
       if(res.ret && res.data) {
         const data = res.data
         this.swiperList = data.swiperList
         this.iconList = data.iconList
         this.recommendList = data.recommendList
         this.weekendList = data.weekendList
       }
       console.log(res)
     }
  },
  mounted () {
      this.getHomeInfo()
      this.lastCity = this.city
  },
  activated () {
      if(this.lastCity !== this.city) {
        this.lastCity = this.city
        this.getHomeInfo()
      }
  }
}
</script>

<style>
  
</style>
