<template>
  <div class="cart_control">
    <transition name="show">
      <i class="cart_decrease iconfont icon-chiping" @click.stop="decrease(food)" v-show="food.count && food.count !== 0"></i>
    </transition>
    <span class="food_count" v-show="food.count && food.count !== 0">{{food.count}}</span>
    <i class="cart_add iconfont icon-roundaddfill" @click.stop="add($event,food)"></i>
  </div>
</template>

<script>
export default {
  props: {
    food: {}
  },
  methods: {
    decrease (food) {
      if (food.count) {
        food.count--
      }
    },
    add (event, food) {
      // console.log(event.target.getBoundingClientRect())
      if (!food.count) {
        this.$set(food, 'count', 1)
      } else {
        this.food.count++
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.show-enter-active,.show-leave-active {
  transition: all .5s;
}
.show-enter,.show-leave-to {
  opacity: 0;
  transform: translateX(56px) rotateZ(180deg)
}
.cart_control{
  position: fixed;
  right: 18px;
  display: inline-block;
  width:80px;height: 24px;
  text-align: center;
  .cart_decrease,.cart_add{
    position: relative;
    top:-4px;
    font-size: 24px;
    line-height: 24px;
    color:rgb(0,160,220);
    padding:4px;
    vertical-align: bottom;
  }
  .food_count{
    font-size: 10px;
    color: rgb(147,153,159);
    line-height: 24px;
    vertical-align: top;
  }
  .cart_decrease{
    float: left;
  }
  .cart_add{
    float: right;
  }
}

</style>
