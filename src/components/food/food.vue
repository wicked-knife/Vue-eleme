<template>
  <transition name='slide'>
    <div class="food" v-show="isShow" ref="food-warpper">
      <div id="warpper">
        <i class="iconfont icon-xiangzuo1 back" @click="_close"></i>
        <div id="image_warpper">
          <img :src="selectedFood.image" id="food_image">
        </div>
        <div id="food_title_warpper">
          <h3 id="food_title">{{selectedFood.name}}</h3>
          <span id="food_sell">月售{{selectedFood.sellCount}}份</span>
          <span id="food_ratings">好评率{{selectedFood.rating}}%</span>
          <div id="price_warpper">
            <span id="food_price">￥{{selectedFood.price}}</span>
            <span id="food_oldprice" v-if="selectedFood.oldPrice">￥{{selectedFood.oldPrice}}</span>
            <div id="control_warpper">
              <div id="for_cartcontrol" v-if="selectedFood.count > 0">
                <cartControl :food="selectedFood"></cartControl>
              </div>
              <div id="add_cart" v-if="!selectedFood.count || selectedFood.count === 0" @click="_add_cart">加入购物车</div>
            </div>
          </div>
        </div>
        <split></split>
        <div id="food_desc_warpper" v-if="selectedFood.info">
          <h3>商品介绍</h3>
          <p id="food_desc">{{selectedFood.info}}</p>
        </div>
        <split v-if="selectedFood.info"></split>
        <div id="food_ratings_warpper">
          <div id="food_ratings_head">
            <h3>商品评价</h3>
            <div id="ratings_type_warpper" class="clearfix">
              <div class="rating_type" id="all_rating" @click="_toggle_all" :class="{'active': isAll}">全部 <span>{{selectedFood.ratings.length}}</span></div>
              <div class="rating_type" id="good_rating" @click="_toggle_good" :class="{'active': isGood}">推荐 <span>{{good_ratings.length}}</span></div>
              <div class="rating_type" id="bad_rating" @click="_toggle_bad" :class="{'active': isBad}">吐槽 <span>{{bad_ratings.length}}</span></div>
            </div>
          </div>
          <div id="only_content">
            <i class="iconfont icon-gou" :class="{'active': isContent}" @click="_toggleIsContent"></i>
            <span>只看有内容的评价</span>
          </div>
        </div>
        <div id="rating_items" v-if="selectedFood.ratings.length > 0">
          <ul>
            <li class="rating_item" v-for="(rating, index) in all_ratings" :key="index">
              <div class="rating_info">
                <span class="rating_time">{{timeFormat(rating.rateTime)}}</span><img :src="rating.avatar" class="rating_avatar">
                <span class="rating_id">{{rating.username}}</span>
              </div>
              <div class="rating_content">
                <i class="iconfont icon-zan good" v-if="!rating.rateType"></i><i class="iconfont icon-cai bad" v-else></i>
                <span class="rating_comment">{{rating.text}}</span>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import split from '@/components/split/split'
import cartControl from '@/components/cartControl/cartControl'
import BetterScroll from 'better-scroll'
export default {
  props: {
    selectedFood: {}
  },
  components: {
    split,
    cartControl
  },
  data () {
    return {
      isShow: false,
      food_warpper: {},
      isContent: true,
      isGood: false,
      isBad: false,
      isAll: true
    }
  },
  methods: {
    show_food () {
      this.isShow = true
    },
    _close () {
      this.isShow = false
      this._init_data()
    },
    _toggleIsContent () {
      this.isContent = !this.isContent
    },
    _add_cart () {
      if (!this.selectedFood.count) {
        this.$set(this.selectedFood, 'count', 1)
      } else {
        this.selectedFood.count++
      }
    },
    _toggle_all () {
      this.isAll = !this.isAll
      this.isGood = false
      this.isBad = false
    },
    _toggle_good () {
      this.isGood = !this.isGood
      this.isAll = false
      this.isBad = false
    },
    _toggle_bad () {
      this.isBad = !this.isBad
      this.isAll = false
      this.isGood = false
    },
    _init_data () {
      this.isContent = true
      this.isGood = false
      this.isBad = false
      this.isAll = true
    },
    timeFormat (time) {
      var date = new Date(time)
      var year = date.getFullYear()
      var month = date.getMonth() + 1
      if (month < 10) {
        month = '0' + month
      }
      var day = date.getDate()
      if (day < 10) {
        day = '0' + day
      }
      var hour = date.getHours()
      var minute = date.getMinutes()
      return `${year}-${month}-${day} ${hour}:${minute}`
    }
  },
  mounted () {
    this.food_warpper = new BetterScroll(this.$refs['food-warpper'], {click: true})
  },
  computed: {
    good_ratings () {
      return this.selectedFood.ratings.filter((rating) => {
        return rating.rateType === 0
      })
    },
    bad_ratings () {
      return this.selectedFood.ratings.filter((rating) => {
        return rating.rateType === 1
      })
    },
    all_ratings () {
      if (this.isContent) {
        if (this.isAll) {
          return this.selectedFood.ratings.filter((rating) => {
            return rating.text
          })
        }
        if (this.isGood) {
          return this.good_ratings.filter((rating) => {
            return rating.text
          })
        }
        if (this.isBad) {
          return this.bad_ratings.filter((rating) => {
            return rating.text
          })
        }
      } else {
        if (this.isAll) {
          return this.selectedFood.ratings.filter((rating) => {
            return rating
          })
        }
        if (this.isGood) {
          return this.good_ratings.filter((rating) => {
            return rating
          })
        }
        if (this.isBad) {
          return this.bad_ratings.filter((rating) => {
            return rating
          })
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../../common/mixin.scss";
.slide-enter,
.slide-leave-to {
  transform: translateX(100%);
}
.slide-enter-to,
.slide-leave {
  transform: translateX(0);
}
.slide-enter-active,
.slide-leave-active {
  transition: all 0.4s;
}
.food {
  position: fixed;
  width: 100%;
  left: 0;
  top: 0;
  bottom: 46px;
  background: #fff;
  overflow: hidden;
  .back {
    font-size: 30px;
    color: #fff;
    position: absolute;
    padding: 10px;
    left: 10px;
    top: 10px;
    z-index: 1;
  }
  #image_warpper {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
    #food_image {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }
  }
  #food_title_warpper {
    padding: 18px;
    font-size: 0;
    #food_title {
      font-size: 14px;
      font-weight: 700;
      color: rgb(7, 17, 27);
      margin-bottom: 8px;
    }
    #food_sell,
    #food_ratings {
      font-size: 10px;
      color: rgb(147, 153, 159);
    }
    #food_sell {
      margin-right: 12px;
    }
    #price_warpper {
      margin-top: 18px;
      #food_price {
        font-size: 14px;
        color: rgb(240, 20, 20);
        font-weight: 700;
        margin-right: 12px;
        margin-top: 18px;
      }
      #food_oldprice {
        font-size: 10px;
        font-weight: 700;
        color: rgb(149, 153, 159);
        text-decoration: line-through;
      }
      #control_warpper {
        float: right;
        position: relative;
        top: -4px;
        right: 0;
        #add_cart {
          font-size: 10px;
          color: #fff;
        background-color: rgb(0, 160, 220);
        padding: 6px 12px;
        border-radius: 12px;
        }
        #for_cartcontrol {
          position: absolute;
          top: -4px;
        }
      }
    }
  }
  #food_desc_warpper {
    padding: 18px;
    color: rgb(7, 17, 27);
    h3 {
      font-size: 14px;
      font-weight: 700;
      margin-bottom: 6px;
    }
    #food_desc {
      font-size: 12px;
      line-height: 24px;
      color: rgb(77, 85, 93);
      padding: 0 8px;
    }
  }
  #food_ratings_warpper {
    padding: 18px 18px 12px 18px;
    @include border-bottom-1px (rgba(7,17,27,.2));
    #food_ratings_head {
      padding-bottom: 18px;
      @include border-bottom-1px (rgba(7,17,27,.2));
    }
    h3 {
      font-size: 14px;
      font-weight: 700;
      margin-bottom: 6px;
    }
    #ratings_type_warpper {
      margin-top: 12px;
      .rating_type {
        padding: 8px 16px;
        float: left;
        font-size: 12px;
        text-align: center;
        margin-right: 8px;
        color: rgb(77, 83, 89);
        border-radius: 2px;
        span {
          font-size: 8px;
          margin-left: 4px;
        }
      }
      #all_rating {
        background-color: rgba(0, 160, 220, 0.2);
        &.active {
          background-color: rgb(0, 160, 220);
          color: #fff;
        }
      }
      #good_rating {
        background-color: rgba(0, 160, 220, 0.2);
        &.active {
          background-color: rgb(0, 160, 220);
          color: #fff;
        }
      }
      #bad_rating {
        background-color: rgba(77, 85, 93, 0.2);
        &.active {
          background-color: rgb(77, 85, 93);
          color: #fff;
        }
      }
    }
  }
  #only_content {
    margin-top: 12px;
    color: rgb(147, 153, 159);
    i {
      font-size: 24px;
      &.active{
        color:rgb(0,200,80)
      }
    }
    span {
      font-size: 12px;
      line-height: 24px;
      margin-left: 4px;
      vertical-align: top;
    }
  }
  #rating_items {
    .rating_item {
      padding: 16px 18px;
      position: relative;
      @include border-bottom-1px (rgba(7,17,27,.2));
      .rating_time,
      .rating_id {
        font-size: 10px;
        color: rgb(147, 153, 159);
        line-height: 20px;
      }
      .rating_id {
        float: right;
      }
      .rating_avatar {
        width: 12px;
        height: 12px;
        border-radius: 50%;
        float: right;
        margin-top: 3px;
        margin-left: 6px;
      }
      .rating_content{
        i{
          font-size: 12px;
          line-height: 24px;
          &.good{
            color:rgb(0,160,220)
          }
          &.bad{
            color:rgb(147,153,159)
          }
        }
        .rating_comment{
          font-size: 12px;
          color:rgb(7,17,27);
        }
      }
    }
  }
}
</style>
