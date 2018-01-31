<template>
<div id="cart-warpper">
  <div class="cart">
    <div class="cart_left" @click="toggle_Cart">
      <div class="cart_logo">
        <i class="iconfont icon-cart_fill_light" :class="{'cart_active': total_count>=0}"></i>
        <div class="total_count" v-show="total_count">
          {{total_count}}
        </div>
      </div>
      <span class="total_price">
        ￥{{total_price}}
      </span>
      <span class="delivery_price">还需要配送费用￥{{seller.deliveryPrice}}元</span>
    </div>
    <div class="cart_right" :class="{'cart_active': total_price >= seller.minPrice}" @click="pay">
      {{isOK}}
    </div>
    <transition name="fold">
      <div id="cart_detail" v-show="isShow && cart_list.length > 0">
        <div id="cart_detail_title">
          <span>购物车</span>
          <span id="empty_cart" @click.stop="empty_cart">清空</span>
        </div>
        <div id="cart_detail_list" ref="cart-warpper">
          <ul>
            <li v-if="cart_list" v-for="(food, index) in cart_list" :key="index" class="cart_item">
              <div class="for_bodder">
                <span>{{food.name}}</span>
                <cartControl :food="food" class="for_position"></cartControl>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
  <div id="detail-mask" v-show="isShow && cart_list.length > 0" @click="hide_mask">
  </div>
  </div>
</template>

<script>
import cartControl from '@/components/cartControl/cartControl'
import BetterScroll from 'better-scroll'
export default {
  props: {
    foodList: {},
    seller: {}
  },
  components: {
    cartControl
  },
  data () {
    return {
      cartScroll: {},
      isShow: false
    }
  },
  computed: {
    total_count: function () {
      var count = 0
      this.foodList.forEach(food => {
        if (food.count) {
          count += food.count
        }
      })
      return count
    },
    total_price: function () {
      var price = 0
      this.foodList.forEach(food => {
        if (food.count) {
          price += food.price * food.count
        }
      })
      return price
    },
    isOK: function () {
      if (!this.total_price) {
        return `￥20起送`
      }
      if (this.total_price > 0 && this.total_price < this.seller.minPrice) {
        let price = this.seller.minPrice - this.total_price
        return `还差${price}元起送`
      }
      if (this.total_price >= this.seller.minPrice) {
        return `去结算`
      }
    },
    cart_list: function () {
      return this.foodList.filter(function (food) {
        if (food.count) {
          return food
        }
      })
    }
  },
  watch: {
    cart_list: function (newvalue, oldvalue) {
      this.cartScroll.refresh()
    }
  },
  methods: {
    show () {
      console.log(this.cart_list)
    },
    empty_cart () {
      this.cart_list.forEach(food => {
        food.count = 0
      })
      this.isShow = false
    },
    _initScroll () {
      this.cartScroll = new BetterScroll(this.$refs['cart-warpper'], {
        probeType: 3,
        click: true
      })
    },
    toggle_Cart () {
      if (this.cart_list.length > 0) {
        this.isShow = !this.isShow
      }
    },
    hide_mask () {
      this.isShow = false
    },
    pay () {
      if (this.total_price > this.seller.minPrice) {
        alert(`支付${this.total_price}`)
      }
    }
  },
  mounted () {
    this.$nextTick(this._initScroll())
  }
}
</script>

<style lang="scss" scoped>
@import "../../common/mixin.scss";
$font-color: rgba(255, 255, 255, 0.4);
.fold-enter,
.fold-leave-to {
  transform: translateY(100%);
  opacity: 0;
}
.fold-enter-active,
.fold-leave-active {
  transition: all 0.3s;
}
.fold-enter-to,
.fold-leave {
  transform: translateY(0);
}
#cart-warpper{
    #detail-mask{
    position: fixed;
    width: 100%;
    height: 100%;
    background:rgba(7,17,27,.6);
    top:0;left:0;
    z-index: 1;
  }
}

.cart {
  display: flex;
  width: 100%;
  height: 46px;
  position: fixed;
  bottom: 0;
  left: 0;
  background: #141d27;
  color: $font-color;
  z-index: 2;
  .cart_left {
    flex: 1;
    .cart_logo {
      float: left;
      display: inline-block;
      position: relative;
      width: 48px;
      height: 48px;
      padding: 6px;
      border-radius: 50%;
      background: #141d27;
      margin-left: 18px;
      bottom: 12px;
      text-align: center;
      .total_count {
        background-color: rgb(240, 20, 20);
        color: #fff;
        font-size: 10px;
        line-height: 12px;
        text-align: center;
        font-weight: 700;
        position: absolute;
        top: 0;
        right: 4px;
        padding: 3px 6px;
        border-radius: 10px;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
      }
      i {
        display: inline-block;
        width: 48px;
        height: 48px;
        font-size: 24px;
        line-height: 48px;
        border-radius: 50%;
        background: rgb(43, 52, 60);
        &.cart_active {
          background: rgb(0, 160, 220);
          color: #fff;
        }
      }
    }
    .total_price {
      float: left;
      font-size: 16px;
      font-weight: 700;
      line-height: 30px;
      margin-top: 8px;
      padding-right: 12px;
      border-right: 1px solid rgba(255, 255, 255, 0.1);
    }
    .delivery_price {
      font-size: 10px;
      line-height: 46px;
      padding-left: 12px;
    }
  }
  .cart_right {
    flex: 0 0 105px;
    width: 105px;
    background-color: rgb(43, 51, 59);
    font-size: 12px;
    font-weight: 700;
    line-height: 46px;
    text-align: center;
    &.cart_active {
      background-color: rgb(0, 180, 60);
      color: #fff;
    }
  }
  #cart_detail {
    position: fixed;
    max-height: 258px;
    width: 100%;
    bottom: 46px;
    color: rgb(7, 17, 27);
    z-index: -100;
    #cart_detail_title {
      height: 40px;
      background-color: #f3f5f7;
      line-height: 40px;
      font-size: 14px;
      padding: 0 18px;
      @include border-bottom-1px (rgba(7,17,27,.1));
      #empty_cart {
        font-size: 12px;
        color: rgb(0, 160, 220);
        float: right;
      }
    }
    #cart_detail_list {
      max-height: 218px;
      overflow: hidden;
      background-color: #fff;
      .cart_item {
        height: 48px;
        background-color: #f3f5f7;
        padding: 0 18px;
        font-size: 14px;
        font-weight: 700;
        line-height: 48px;
        background-color: #fff;
        position: relative;
        .for_bodder {
          @include border-bottom-1px (rgba(7,17,27,.1));
        }
        .for_position {
          margin-top: 13px;
        }
      }
    }
  }
}

</style>
