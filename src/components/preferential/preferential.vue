<template>
  <div id="preferential" :class="[logo_size[size], logo_type[type], isOppo[type]]">
  </div>
</template>

<script>
export default {
  props: {
    type: {},
    size: {},
    oppo: {}
  },
  data () {
    return {
      logo_type: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      logo_size: {small: 'small', medium: 'medium', large: 'large'}
    }
  },
  computed: {
    isOppo: function () {
      if (this.oppo) {
        return ['decrease_oppo', 'discount_oppo', 'special_oppo', 'invoice', 'guarantee']
      } else {
        return this.logo_type
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  #preferential{
    display: inline-block;
  }
.small{
  width: 12px;height: 12px;
  background-size: 12px 12px;
}
.medium{
  width: 16px;height: 16px;
  background-size: 16px 16px;
}
.decrease {
  background-image: url("./decrease_1@2x.png");
}
.decrease_oppo {
  background-image: url("./decrease_4@3x.png") !important;
  margin-right: 2px;
}
.discount {
  background-image: url("./discount_1@2x.png");
}
.discount_oppo {
  background-image: url("./discount_4@3x.png") !important;
  margin-right: 2px;
}
.special {
  background-image: url("./special_1@2x.png");
}
.special_oppo {
  background-image: url("./special_4@3x.png") !important;
  margin-right: 2px;
}
.invoice {
  background-image: url("./invoice_1@2x.png");
}
.guarantee {
  background-image: url("./guarantee_1@2x.png");
}

</style>
