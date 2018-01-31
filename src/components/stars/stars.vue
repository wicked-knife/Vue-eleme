<template>
  <div id="stars">
    <i v-for="star in stars" :key="star.index" :class="star" class="star"></i>
  </div>
</template>

<script>
const LENGTH = 5
export default {
  props: {
    size: {},
    score: {}
  },
  computed: {
    stars () {
      // 根据传入的分数处理为正确的分数
      // const STARS = []
      // const SCORE = Math.floor(this.score * 2) / 2
      // var isDecimal = false
      // const full = Math.floor(SCORE)
      // 判断是否为小数
      // if (SCORE % 1 !== 0) {
      //   isDecimal = true
      // }
      // const LENGTH = 5
      // 根据分数遍历每一个星星
      // for (let index = 0; index < full; index++) {
      //   let star = {}
      //   star.className = `star_${this.size} star_on`
      //   STARS.push(star)
      // }
      // if (isDecimal) {
      //   let star = {}
      //   star.className = `star_${this.size} star_half`
      //   STARS.push(star)
      //   for (let index = full + 1; index < LENGTH; index++) {
      //     let star = {}
      //     star.className = `star_${this.size} star_off`
      //     STARS.push(star)
      //   }
      // } else {
      //   for (let index = full; index < LENGTH; index++) {
      //     let star = {}
      //     star.className = `star_${this.size} star_off`
      //     STARS.push(star)
      //   }
      // }
      // return STARS
      const SCORE = Math.floor(this.score * 2) / 2
      const FULL = Math.floor(SCORE)
      const isDecimal = SCORE % 1 !== 0
      const on = `star_${this.size} star_on`
      const half = `star_${this.size} star_half`
      const off = `star_${this.size} star_off`
      let STARS = []
      for (let index = 0; index < FULL; index++) {
        STARS.push(on)
      }
      if (isDecimal) {
        STARS.push(half)
      }
      while (STARS.length !== LENGTH) {
        STARS.push(off)
      }
      return STARS
    }
  }
}
</script>

<style lang="scss" scoped>
#stars {
  display: inline-block;
}
.star {
  display: inline-block;
  background-repeat: no-repeat;
}
.star_48 {
  width: 20px;
  height: 20px;
  margin-right: 22px;
  background-size: 20px 20px;
  &:last-child {
    margin-right: 0;
  }
  &.star_on {
    background-image: url("./star48_on@2x.png");
  }
  &.star_half {
    background-image: url("./star48_half@2x.png");
  }
  &.star_off {
    background-image: url("./star48_off@2x.png");
  }
}
.star_36 {
  width: 15px;
  height: 15px;
  margin-right: 6px;
  background-size: 15px 15px;
  &:last-child {
    margin-right: 0;
  }
  &.star_on {
    background-image: url("./star36_on@2x.png");
  }
  &.star_half {
    background-image: url("./star36_half@2x.png");
  }
  &.star_off {
    background-image: url("./star36_off@2x.png");
  }
}
.star_24 {
  width: 10px;
  height: 10px;
  margin-right: 3px;
  background-size: 10px 10px;
  &:last-child {
    margin-right: 0;
  }
  &.star_on {
    background-image: url("./star24_on@2x.png");
  }
  &.star_half {
    background-image: url("./star24_half@2x.png");
  }
  &.star_off {
    background-image: url("./star24_off@2x.png");
  }
}
</style>
