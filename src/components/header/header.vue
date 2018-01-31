<template>
  <div id="header">
    <div id="content_warpper">
      <div id="header_container">
        <div id="header_content">
          <img :src="seller.avatar" id="avatar">
          <div id="header_textbox">
            <img src="./brand@2x.png" id="brand">
            <span id="seller_name">{{ seller.name }}</span><br>
            <span id="delivery">{{ seller.description }}/{{ seller.deliveryTime }}分钟送达</span>
            <preferential v-if="seller.supports" :type="seller.supports[0].type" :size="'small'"></preferential>
            <span v-if="seller.supports" id="preferential_text">{{ seller.supports[0].description }}</span>
            <div id="supports" @click="showDetail">
              <span v-if="seller.supports" id="supports_count">{{seller.supports.length}}个</span>
              <i class="iconfont icon-xiangyou1"></i>
            </div>
          </div>
        </div>
      </div>
      <div id="bulletin" @click="showDetail">
        <img src="./bulletin@2x.png" id="bulletin_logo">
        <span id="bulletin_text">{{ seller.bulletin }}</span>
        <i class="iconfont icon-xiangyou1"></i>
      </div>
      <div id="bg_blur" v-if="seller.avatar">
        <img :src="seller.avatar">
      </div>
    </div>
    <div id="tab">
      <div class="tab_item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab_item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab_item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <transition name="fade">
      <div id="detail" v-show="isDetailShow">
        <div id="detail_content">
          <h1 id="detail_title">{{seller.name}}</h1>
          <div class="stars_warpper">
            <stars :score="seller.score" :size="48"></stars>
          </div>
          <lineTitle :title="'优惠信息'"></lineTitle>
          <ul class="support_list">
            <li v-for="item in seller.supports" :key="item.id">
              <preferential :type="item.type" :size="'medium'" class="detail_logo"></preferential>
              <span class="support_desc">{{item.description}}</span>
            </li>
          </ul>
          <lineTitle :title="'商家公告'"></lineTitle>
          <p id="detail_bulletin">{{seller.bulletin}}</p>
        </div>
        <div id="detail_close" @click="closeDetail">
          <i class="iconfont icon-guanbi1"></i>
        </div>
      </div>
    </transition>

  </div>
</template>

<script>
import stars from '@/components/stars/stars'
import lineTitle from '@/components/header/line_title'
import preferential from '@/components/preferential/preferential'
export default {
  props: {
    seller: {}
  },
  components: {
    stars,
    lineTitle,
    preferential
  },
  data () {
    return {
      isDetailShow: false
    }
  },
  methods: {
    showDetail () {
      this.isDetailShow = true
    },
    closeDetail () {
      this.isDetailShow = false
    }
  }
}
</script>

<style scoped lang="scss">
@import '../../common/mixin.scss';
#header{
  width: 100%;
  overflow: hidden;
  position: relative;
  #content_warpper {
    position: relative;
  }
}
#tab {
  display: flex;
  @include border-bottom-1px (rgba(17,27,37,.2));
  height: 40px;
  font-size: 14px;
  .tab_item {
    flex: 1;
    line-height: 40px;
    text-align: center;
    a {
      display: block;
      text-decoration: none;
    }
  }
}
#header_container {
  height: 108px;
  width: 100%;
  box-sizing: border-box;
  padding: 24px 12px 0 24px;
  background-color: rgba(7, 17, 27, 0.5);
  color: white;
  font-weight: 200;
  #header_content {
    position: relative;
    #avatar {
      width: 64px;
      height: 64px;
      float: left;
      border-radius: 2px;
    }
    #header_textbox {
      width: 68.8%;
      float: left;
      margin-left: 16px;
    }
    #brand {
      width: 30px;
      height: 18px;
      vertical-align: top;
    }
    #seller_name {
      font-size: 16px;
      vertical-align: top;
    }
    #delivery {
      display: block;
      font-size: 12px;
      line-height: 14px;
      margin-top: 8px;
      margin-bottom: 8px;
    }
    #preferential_text {
      font-size: 10px;
      vertical-align: middle;
    }
    #supports {
      height: 24px;
      box-sizing: border-box;
      padding: 0 6px;
      background: rgba(0, 0, 0, 0.2);
      position: absolute;
      right: 0;
      top: 44px;
      font-size: 10px;
      border-radius: 12px;
      #supports_count {
        line-height: 24px;
      }
      i {
        font-size: 10px;
        line-height: 24px;
        margin-left: 2px;
      }
    }
  }
}

#bulletin {
  position: relative;
  height: 28px;
  width: 100%;
  box-sizing: border-box;
  padding-left: 12px;
  padding-right: 20px;
  background: rgba(7, 17, 27, 0.6);
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  color: white;
  #bulletin_logo {
    width: 22px;
    height: 12px;
    position: relative;
    top: 1px;
  }
  #bulletin_text {
    width: 100%;
    line-height: 28px;
    font-size: 10px;
    color: #fff;
  }
  i {
    position: absolute;
    font-size: 10px;
    left: 356px;
    top: 8px;
  }
}
#bg_blur {
  width: 100%;
  height: 136px;
  position: absolute;
  left: 0;
  top: 0;
  z-index: -1;
  background-repeat: no-repeat;
  background-size: 100% 100%;
  filter: blur(10px);
  overflow: hidden;
  img {
    width: 100%;
    height: 100%;
  }
}
#detail {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 9;
  background-color: rgba(7, 17, 27, 0.8);
  overflow: auto;
  padding: 64px 36px 36px 32px;
  display: flex;
  flex-flow: column;
  #detail_content {
    flex: 1;
    color: white;
    #detail_title {
      font-size: 16px;
      font-weight: 700;
      text-align: center;
      margin: 0;
    }
    .stars_warpper {
      margin-top: 16px;
      text-align: center;
      margin-bottom: 28px;
    }
    .support_list {
      margin: 24px 0 28px 0;
      padding: 0;
      li {
        margin: 0 0 12px 0;
        padding: 0;
        list-style: none;
        display: block;
        .detail_logo{
          margin-left: 12px;
        }
        .support_desc {
          font-size: 12px;
          font-weight: 200;
          margin-left: 6px;
          color: white;
          position: relative;
          top: -3px;
        }
      }
    }
    #detail_bulletin {
      font-size: 12px;
      line-height: 24px;
      padding: 0 12px;
      margin-top: 24px;
    }
  }
  #detail_close {
    flex: 0;
    text-align: center;
    i {
      color: rgba(255, 255, 255, 0.5);
      font-size: 32px;
    }
  }
}
.fade-enter-active {
  transition: opacity .5s;
  opacity: 1;
}
.fade-leave-active {
  transition: opacity .5s;
  opacity: 0;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
</style>
