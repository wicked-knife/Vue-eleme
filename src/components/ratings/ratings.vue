<template>
  <div id="ratings" ref="better-scroll-hook">
    <div>
      <div id="ratings_overview">
        <div id="overview_left">
          <span>{{seller.score}}</span>
          <span>综合评分</span>
          <span>高于周边商家{{seller.rankRate}}%</span>
        </div>
        <div id="overview_right">
          <div>
            <span>服务态度</span>
            <stars :score="seller.serviceScore" :size="36"></stars>
            <span class="tiny_score">{{seller.serviceScore}}</span>
          </div>
          <div>
            <span>商品评分</span>
            <stars :score="seller.foodScore" :size="36"></stars>
            <span class="tiny_score">{{seller.foodScore}}</span>
          </div>
          <div>
            <span>送达时间</span>
            <span id="delivery_time">{{seller.deliveryTime}}分</span>
          </div>
        </div>
      </div>
      <split></split>
      <div id="seller_ratings_warpper">
        <div id="seller_ratings_header" class="clearfix">
          <div class="rating_type" id="all_rating" @click="_toggle_all" :class="{'active':isAll}">
            全部
            <span>{{ratings.length}}</span>
          </div>
          <div class="rating_type" id="good_rating" @click="_toggle_good" :class="{'active':isGood}">
            满意
            <span>{{good_ratings.length}}</span>
          </div>
          <div class="rating_type" id="bad_rating" @click="_toggle_bad" :class="{'active':isBad}">
            不满意
            <span>{{bad_ratings.length}}</span>
          </div>
        </div>
      </div>
      <div id="content_only">
        <i class="iconfont icon-gou" @click="_toggleIsContent" :class="{'active': isContent}"></i>
        <span>只看有内容的评价</span>
      </div>
      <div id="seller_ratings_warpper">
        <ul>
          <li class="seller_rating_item" v-for="(rating, index) in all_ratings" :key="index">
            <div class="item_head clearfix">
              <img :src="rating.avatar" class="rating_avatar">
              <div>
                <span class="rating_id">{{rating.username}}</span>
                <span class="rating_time">{{timeFormat(rating.rateTime)}}</span>
              </div>
              <div>
                <stars :score="rating.score" :size="24"></stars>
                <span class="rating_delivery">{{rating.deliveryTime}}分钟送达</span>
              </div>
            </div>
            <div class="rating_comment" v-if="rating.text">
              {{rating.text}}
            </div>
            <div class="rating_bottom clearfix">
              <i class="iconfont icon-zan" v-show="!rating.rateType"></i>
              <i class="iconfont icon-cai" v-show="rating.rateType"></i>
              <ul class="recommend_dish_list clearfix" v-if="rating.recommend.length > 0">
                <li class="recommend_dish" v-for="(dish, index) in rating.recommend" :key="index">{{dish}}</li>
              </ul>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import stars from '@/components/stars/stars'
import split from '@/components/split/split'
import BetterScroll from 'better-scroll'
export default {
  props: {
    ratings: {},
    seller: {}
  },
  components: {
    stars,
    split
  },
  data () {
    return {
      scrollObj: {},
      isContent: true,
      isGood: false,
      isBad: false,
      isAll: true
    }
  },
  computed: {
    good_ratings () {
      return this.ratings.filter((rating) => {
        return rating.rateType === 0
      })
    },
    bad_ratings () {
      return this.ratings.filter((rating) => {
        return rating.rateType === 1
      })
    },
    all_ratings () {
      if (this.isContent) {
        if (this.isAll) {
          return this.ratings.filter((rating) => {
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
          return this.ratings.filter((rating) => {
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
  },
  methods: {
    _toggleIsContent () {
      this.isContent = !this.isContent
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
    this.$nextTick(() => {
      this.scrollObj = new BetterScroll(this.$refs['better-scroll-hook'], {click: true})
    })
  }
}
</script>

<style lang="scss" scoped>
@import "../../common/mixin.scss";
.clearfix:after {
  content: "";
  display: table;
  clear: both;
}
#ratings {
  position: absolute;
  width: 100%;
  top: 174px;
  left: 0;
  bottom: 0;
  overflow: hidden;
  #ratings_overview {
    display: flex;
    padding: 18px 0;
    #overview_left {
      flex: 0 0 138px;
      width: 138px;
      border-right: 1px solid rgba(7, 17, 27, 0.2);
      span {
        display: block;
        text-align: center;
        &:first-child {
          color: rgb(255, 153, 0);
          font-size: 24px;
          margin-top: 6px;
          margin-bottom: 10px;
        }
        &:nth-child(2) {
          font-size: 12px;
          color: rgb(7, 17, 27);
          margin-bottom: 10px;
        }
        &:last-child {
          font-size: 10px;
          color: rgb(147, 153, 159);
        }
      }
    }
    #overview_right {
      margin-left: 24px;
      flex: 1;
      div {
        &:first-child{
          margin-top: 2px;
        }
        span {
          font-size: 12px;
          color: rgb(7, 17, 27);
          line-height: 18px;
          vertical-align: top;
          margin-right: 12px;
          @media screen and (max-width: 360px) {
            margin-right: 3px;
          }
        }
        &:not(:last-child) {
          margin-bottom: 6px;
        }
        #delivery_time {
          color: rgb(147, 153, 159);
        }
        .tiny_score {
          color: rgb(255, 153, 0);
          font-size: 10px;
          margin-left: 12px;
          @media screen and (max-width: 360px) {
            margin-left: 3px;
          }
        }
      }
      @media screen and (max-width: 360px) {
        margin-left: 9px;
      }
    }
  }
  #seller_ratings_warpper {
    padding: 18px 18px 0 18px;
    #seller_ratings_header {
      padding-bottom: 46px;
      @include border-bottom-1px (rgba(7,17,27,.2));
      .rating_type {
        padding: 8px 12px;
        border-radius: 2px;
        margin-right: 8px;
        float: left;
        font-size: 12px;
        margin-right: 8px;
        span {
          font-size: 8px;
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
  #content_only {
    padding: 12px 18px;
    font-size: 12px;
    color: rgb(147, 153, 159);
    line-height: 24px;
    @include border-bottom-1px (rgba(7,17,27,.2));
    i {
      font-size: 24px;
      margin-right: 4px;
      &.active {
        color: rgb(0, 200, 80);
      }
    }
    span {
      vertical-align: top;
    }
  }
}
#seller_ratings_warpper {
  .seller_rating_item {
    @include border-bottom-1px (rgba(7,17,27,.2));
    padding-bottom: 18px;
    padding-top: 18px;
    .item_head {
      .rating_avatar {
        width: 28px;
        height: 28px;
        float: left;
        margin-right: 12px;
        border-radius: 50%;
      }
      div {
        position: relative;
        top: -3px;
      }
      .rating_time {
        font-size: 10px;
        color: rgb(147, 153, 159);
        float: right;
        position: relative;
        top: 3px;
      }
      .rating_id {
        font-size: 10px;
        color: rgb(7, 17, 27);
      }
      .rating_delivery {
        font-size: 10px;
        color: rgb(147, 153, 159);
        vertical-align: top;
        position: relative;
        top: 1px;
      }
    }
    .rating_comment {
      font-size: 12px;
      color: rgb(7, 17, 27);
      line-height: 18px;
      padding-left: 40px;
    }
    .rating_bottom {
      margin-left: 40px;
      .icon-zan {
        color: rgb(0, 160, 220);
        margin-right: 8px;
        float: left;
        margin-top: 5px;
      }
      .icon-cai {
        color: rgb(183, 187, 193);
        margin-right: 8px;
        float: left;
        margin-top: 5px;
      }
      // .recommend_dish_list {
      //   display: inline-block;
        .recommend_dish {
          font-size: 9px;
          color: rgb(147, 153, 159);
          line-height: 16px;
          border: 1px solid rgba(7, 17, 27, 0.1);
          display: inline-block;
          padding: 2px 6px;
          border-radius: 2px;
          float: left;
          position: relative;
          top: 4px;
          margin-right: 8px;
          margin-bottom: 4px;
        }
      // }
    }
  }
}
</style>
