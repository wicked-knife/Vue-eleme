<template>
  <div id="app">
    <common-header :seller="seller"></common-header>
    <keep-alive>
        <router-view   :seller="seller" :goods="goods" :ratings="ratings" v-if="seller.name && goods.length > 0 && ratings.length > 0"/>
    </keep-alive>
  </div>
</template>

<script>
import commonHeader from '@/components/header/header'
import axios from 'axios'
export default {
  name: 'App',
  data () {
    return {
      seller: {},
      goods: {},
      ratings: {}
    }
  },
  // watch: {
  //   seller: function (newval, oldval) {
  //     this.seller = newval
  //   }
  // },
  components: {
    commonHeader
  },
  methods: {
    // getSeller () {
    //   return axios({
    //     url: 'http://localhost:3000/seller',
    //     method: 'get'
    //   })
    // },
    // getGoods () {
    //   return axios({
    //     url: 'http://localhost:3000/goods',
    //     method: 'get'
    //   })
    // },
    // getRatings () {
    //   return axios({
    //     url: 'http://localhost:3000/ratings',
    //     method: 'get'
    //   })
    // }
    // 本地开发用
  },
  created () {
    axios({
      url: 'https://ghostratel.github.io/portfolio/eleme/data.json',
      method: 'get'
    }).then(response => {
      var data = JSON.parse(JSON.stringify(response.data).replace(/http/g, 'https'))
      // 将http资源的图片地址替换为https协议
      this.seller = data.seller
      this.goods = data.goods
      this.ratings = data.ratings
    })
    // axios({
    //   url: 'http://localhost:3000/goods',
    //   method: 'get'
    // }).then(response => {
    //   this.goods = response.data
    //   console.log(response.data)
    // })
    // 本地开发用

    // var _this = this
    // axios.all([_this.getSeller(), _this.getGoods(), _this.getRatings()]).then(axios.spread(function (seller, goods, ratings) {
    //   _this.seller = seller.data
    //   _this.goods = goods.data
    //   _this.ratings = ratings.data
    // }))
    // axios并发多条请求
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'PingFang SC','STHeitiSC-Light','Helvetica-Light','arial','sans-serif';
  color: #2c3e50;
}
a{
  text-decoration: none;
}
ul{
  margin: 0;padding:0;
}
li{
  list-style: none;
  margin: 0;padding:0;
}
h1,h2,h3,h4,h5,h6{
  margin:0;padding:0;
}
</style>
