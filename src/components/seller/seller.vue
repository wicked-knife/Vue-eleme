<template>
  <div id="seller" ref="seller-warpper">
    <div>
      <div id="seller_header">
        <div id="seller_header_top">
          <h3 id="seller_name">{{seller.name}}</h3>
          <div id="sell_count">
            <stars :score="seller.score" :size="36"></stars>
            <span> &nbsp;&nbsp;({{seller.ratingCount}}) &nbsp;&nbsp;&nbsp;月售{{seller.sellCount}}单</span>
          </div>
          <div id="favorite">
            <i class="iconfont icon-like_fill" :class="{'active': isFavorite}" @click="toggle_favorite"></i>
            <span v-if="isFavorite">已收藏</span>
            <span v-else>收藏</span>
          </div>
        </div>
        <div id="seller_header_bottom">
          <div id="seller_header_left">
            <span class="seller-title">起送价</span>
            <div>
              <span>{{seller.minPrice}}</span>元</div>
          </div>
          <div id="seller_header_middle">
            <span class="seller-title">商家配送</span>
            <div>
              <span>{{seller.deliveryPrice}}</span>元</div>
          </div>
          <div id="seller_header_right">
            <span class="seller-title">平均配送时间</span>
            <div>
              <span>{{seller.deliveryTime}}</span>分钟</div>
          </div>
        </div>
      </div>
      <split></split>
      <div id="seller_bulletin">
        <div id="bulletin_text">
          <h3>公告与活动</h3>
          <p>{{seller.bulletin}}</p>
        </div>
        <div id="supports_list" v-if="seller.supports.length">
          <ul>
            <li class="supports_item" v-for="(support, index) in seller.supports" :key="index">
              <preferential :type="support.type" :size="'medium'" :oppo="true"></preferential>
              <span>{{support.description}}</span>
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div id="seller_pics">
        <h3>商家实景</h3>
        <div id="pics_warpper" ref="pics-warpper">
          <div class="clearfix" ref="pics_warpper_hook">
            <img :src="img" v-for="(img, index) in seller.pics" :key="index">
          </div>
        </div>
      </div>
      <split></split>
      <div id="seller_info">
        <h3>商家信息</h3>
        <ul>
          <li v-for="(info, index) in seller.infos" :key="index" class="info_item">
            {{info}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import stars from '@/components/stars/stars'
import split from '@/components/split/split'
import preferential from '@/components/preferential/preferential'
import BetterScroll from 'better-scroll'
// import {urlParse} from '@/common/util'
import store from 'store'
export default {
  props: {
    seller: {}
  },
  data () {
    return {
      seller_warpper: {},
      pics_warpper: {},
      isFavorite: false
    }
  },
  components: {
    stars,
    split,
    preferential
  },
  methods: {
    toggle_favorite () {
      this.isFavorite = !this.isFavorite
      store.set('favorite', this.isFavorite)
    }
  },
  mounted () {
    this.$refs['pics_warpper_hook'].style.width = (120 + 6) * this.seller.pics.length + 'px'
    this.$nextTick(() => {
      this.seller_warpper = new BetterScroll(this.$refs['seller-warpper'], {click: true})
      this.pics_warpper = new BetterScroll(this.$refs['pics-warpper'], {scrollX: true})
    })
    if (store.get('favorite')) {
      this.isFavorite = store.get('favorite')
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../../common/mixin.scss";
#seller {
  position: absolute;
  top: 176px;
  bottom: 0;
  width: 100%;
  overflow: hidden;
  #seller_header {
    padding: 18px 18px 0 18px;
    #seller_header_top {
      @include border-bottom-1px (rgba(7,17,27,.2));
      padding-bottom: 18px;
      position: relative;
      #seller_name {
        font-size: 14px;
        color: rgb(7, 17, 27);
        font-weight: normal;
        margin-bottom: 8px;
      }
      #sell_count {
        font-size: 10px;
        color: rgb(77, 85, 93);
        span {
          position: relative;
          top: -2px;
        }
      }
      #favorite {
        position: absolute;
        right: 0;
        top: 0;
        text-align: center;
        width: 46px;
        i {
          font-size: 26px;
          &.active{
            color: rgb(240,20,20)
          }
        }
        span {
          display: block;
          font-size: 10px;
          color: rgb(77, 85, 93);
          margin-top: 4px;
        }
      }
    }
    #seller_header_bottom {
      display: flex;
      padding: 18px 0;
      #seller_header_left {
        flex: 1;
        text-align: center;
        border-right: 1px solid rgba(7, 17, 27, 0.2);
        .seller-title {
          font-size: 10px;
          color: rgb(147, 153, 159);
          line-height: 20px;
          display: block;
          margin-bottom: 4px;
        }
        div {
          font-size: 10px;
          span {
            font-size: 24px;
          }
        }
      }
      #seller_header_middle {
        flex: 1;
        text-align: center;
        border-right: 1px solid rgba(7, 17, 27, 0.2);
        .seller-title {
          font-size: 10px;
          color: rgb(147, 153, 159);
          line-height: 20px;
          display: block;
          margin-bottom: 4px;
        }
        div {
          font-size: 10px;
          span {
            font-size: 24px;
          }
        }
      }
      #seller_header_right {
        flex: 1;
        text-align: center;
        .seller-title {
          font-size: 10px;
          color: rgb(147, 153, 159);
          line-height: 20px;
          display: block;
          margin-bottom: 4px;
        }
        div {
          font-size: 10px;
          span {
            font-size: 24px;
          }
        }
      }
    }
  }
  #seller_bulletin {
    padding: 18px 18px 0 18px;
    #bulletin_text {
      padding-bottom: 16px;
      @include border-bottom-1px (rgba(7,17,27,.2));
      h3 {
        font-size: 14px;
        font-weight: normal;
        color: rgb(7, 17, 27);
      }
      p {
        color: rgb(240, 20, 20);
        font-weight: 200;
        font-size: 12px;
        line-height: 24px;
        padding: 0 12px;
      }
    }
    #supports_list {
      .supports_item {
        padding: 16px 12px;
        span {
          font-size: 12px;
          font-weight: 200;
          color: rgb(7, 17, 27);
          line-height: 16px;
          vertical-align: top;
        }
        &:not(:last-child) {
          @include border-bottom-1px (rgba(7,17,27,.2));
        }
      }
    }
  }
  #seller_pics {
    padding: 18px 18px 18px 18px;
    h3 {
      font-size: 14px;
      font-weight: normal;
      color: rgb(7, 17, 27);
    }
    #pics_warpper {
      font-size: 0;
      margin-top: 12px;
      white-space: nowrap;
      width: 100%;
      .pics_warpper_hook{
        width: 500px;
      }
      img {
        width: 120px;
        height: 90px;
        margin-right: 6px;
      }
    }
  }
  #seller_info {
    padding: 18px 18px 0 18px;
    h3 {
      font-size: 14px;
      font-weight: normal;
      color: rgb(7, 17, 27);
      padding-bottom: 12px;
      @include border-bottom-1px (rgba(7,17,27,.2));
    }
    .info_item {
      font-size: 12px;
      font-weight: 200;
      color: rgb(7, 17, 27);
      line-height: 16px;
      padding: 16px 12px;
      &:not(:last-child){
        @include border-bottom-1px (rgba(7,17,27,.2));
      }
    }
  }
}
</style>
